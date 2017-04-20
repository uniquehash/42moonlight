## SSH Tunnel Demo
This will be a crash course tutorial in how to set up a "jump tunnel" between users on unix machines.
I'll also be showing how to set this up behind a protected firewall to create an inbound connection when one would be
otherwise impossible.


##### What you will need:
- A command line interface (or TTY) (on your client machine)
- A target machine (The machine that sits behind the firewall)
- An intermediary machine (Something that is always available to connect to)
<br><br>

##### From here they will be referred to as:
- Client
- Target
- Jumper
<br><br>
*Real quick, why? Because Target is behind a firewall that doesn't allow inbound connections. So if we try:*
```
user@client:~$ ssh hit@target
```
*We get an error that Target cannot be reached, or similar. The work around is that Client and Target can both see
Jumper, so we use Jumper as a middleman.*
<br><br>
<br><br>


## First, set up your users
The most common command for linux systems is `adduser`, sometimes with or without modifiers.

```
root@target:~$ adduser hit
```
```
root@jumper:~$ adduser jmp
```
I'm creating the users`hit` for `target` and `jmp` for `jumper`, but you can name yours whatever you like, or even the
same thing for both (for easier management and identification).
You will want to make sure that they have adequate permissions and an appropriate shell to set up at first, though this can be
modified later.
<br><br>
<br><br>

## Second, set up key pairs for authentication
These are more secure than passwords for authentication, and allow for instantaneous connection.

```
[anyuser]@target:~$ ssh-keygen -t rsa
```
```
jmp@jumper:~$ ssh-keygen -t rsa
```
#### Explanation:
- **Any user** can be used to initialize the tunnel, as long as they are allowed to make ssh connections as a client. *Hint:
you can even have it run as a script in the background, and/or launch at startup.*
<br><br>
If you don't already know how to handle keys, I highly recommend looking it up. This crash course will be a
good introduction but nothing beats research and regular usage.
<br><br>
<br><br>

## Third, copy keys from Target to Jumper and make the outbound connection
An easy and secure way to copy RSA keys from one unix machine to another is with `ssh-copy-id`.
- This isn't always available. In those cases use:
- `cat ~/.ssh/id_rsa.pub | ssh [user]@[target.machine] 'echo >> .ssh/authorized_keys'`
<br><br>
```
[anyuser1]@target:~$ ssh-copy-id jmp@jumper
...
[anyuser1]@target:~$ ssh -R [port1]:localhost:[port2] [anyuser2]@jumper
```
#### Explanation:
- **anyuser1** : same user that you set up for the outbound connection.
- **Ports** haven't really been covered yet, but it's good practice to change your `/etc/ssh/sshd_config` file to listen on
a different port. Anyways, with this tunnel, the **Jumper** machine will need to know when a connection is inbound
towards the **Jumper**, or inbound towards the **Target**, so we use a different port.
- **port1** : the port that Jumper listens to in order to *forward to Target* (This can be any port that Jumper is not
already using.
- **localhost** : Target wants connections to be forwarded to itself. 
- **port2** : the port that **Target** listens to for inbound ssh connections (Target's `/etc/ssh/sshd_config`)
- **anyuser2** : Any user can be used to recieve the connection, but they must be a user that can be signed into (aka have a shell that is
not `/bin/false` or equivalent. More on that later.)
<br><br>
<br><br>

## Fourth, copy keys from Jumper to Target (CHECKPOINT REACHED)
At this point, the tunnel is made and you can sign in to your Target computer! Well, after you connect to your Jumper
that is.
<br>
The way that you make a connection to Target from Jumper is that you make a connection from Jumper to Jumper at the
different port we specified earlier.
```
jmp@jumper:~$ ssh-copy-id target@localhost -p [port1]
```
*Remember: port1 is the port Jumper listens to in order to forward.*
<br><br>
Great! Now we can connect to our target. But it's a bit choppy, we have to authenticate twice and I personally don't
want to sit around on the Jumper machine.
```
user@client:~$ ssh jmp@jumper
...
jmp@jumper:~$ ssh hit@localhost -p [port1]
...
hit@target:~$
hit@target:~$ exit
Connection to localhost closed.
jmp@jumper:~$
jmp@jumper:~$ exit
Connection to jumper closed.
user@client:~$
```
*Note: I know that there is a way to simply* `ssh hit@jumper -p [port1]` *from client. I'm working on looking up that
syntax and figuring it out myself.*
<br><br>
<br><br>
## Fifth (and final for the basics), Jump Quietly
This step is going to make the transition from one machine to another seamless and quick. We already set up the key pair
authentication between `jmp@jumper` and `hit@target`, but I advise setting up the keypair for `user@client` and
`jmp@jumper` at this point because beyond here `jmp`'s passwords and keys will not be able to be modified without
modification from `root@jumper` or another administrator or script.
- You're going to need `vim` (or `emacs`, if that's how you roll)
```
root@jumper:~$ vim /etc/passwd
```
```
root:x:0:0:root:/root:/bin/bash
...
...
(lots of system users here)
...
...
someuser:x:1001:1001:,,,:/home/someuser:/bin/bash
jmp:x:1000:1000:,,,:/home/jmp:/bin/bash
```
**Change the last line to the following:**
```
jmp:x:1000:1000:,,,:/home/jmp:/usr/bin/my_script.sh
```
**Now create the following script:**
```
root@jumper:~$ vim /usr/bin/my_script.sh
```
```
#!/bin/bash

ssh hit@localhost -p [port1]
exit
```
**Don't forget to**
```
root@jumper:~$ chmod +x /usr/bin/my_script.sh
```
#### Explanation:
- **/etc/passwd** : A system file used to keep track of users, at this point we are changing the login shell from
`/bin/bash` to `my_script.sh`, effectively restricting the actions that the user `jmp` can do to only connect and then
quit.
- **my_script.sh** : As soon as a terminal is opened by user `jmp`, either by logging in locally or through an ssh
connection, that user's shell environment is replaced by the script we made. Our script immediately goes through the
process of connecting to the Target machine which (if our keys are configured correctly and Target has set up the
outbound tunnel) immediately drops us in the shell of `hit@target`. When the connection is closed from `hit@target`, we
return to our script and immediately execute the `exit` command, which will close our connection to (or terminal in)
`jmp`.
<br><br>
Connection should look something like this:
```
user@client:~$ ssh jmp@jumper
hit@target:~$
hit@target:~$
hit@target:~$ exit
Connection to localhost closed.
Connection to jumper closed.
user@client:~$
```
The only downside to this "gimped" shell of `jmp` is that they no longer have the ability to add or modify their .ssh
keys or password. 
- I'm going to work on overhead scripts that can be run from `hit@target` that will pass through the
connection to update `jmp@jumper` **Bug me until I post them.**

<br><br><br><br><br><br>
- TODO: MOTD and banners
- TODO: Demonstrate the automation of creating a user via shell script
- TODO: Setup key generation for other users via shell script
- TODO: Look up port forwarding in `GatewayPorts` in `/etc/ssh/sshd_config`
- TODO: make sense of the potential for `ssh -R *:[port1]:localhost:[port2] ...`
- TODO: Add an "active users" functionality
- Todo: Add linked key/password management (changing password on Target updates Jumper) (add ability to change
authorized Public Keys on Jumper after gimping)

