#Get Started with Hosting a Ghost Blog on Digital Ocean

**Author:** Chris Renfrow - @crenfrow - mail@chrisrenfrow.me

###Intro
[Ghost](https://ghost.org/developers/) is a lightweight, open-source blogging platform designed with ease-of-use in mind. Customizability is made easy through the [Ghost Marketplace](http://marketplace.ghost.org/themes) where you can download both paid and free themes. Of course, making your own theme is always an option (an option that I would recommend). This tutorial aims to provide all the necessary information to host your own Ghost blog on a Digital Ocean droplet.

**Please note:** This is the 'hard' way of doing it. I recommend doing it this way to learn but if you just want something in a hurry, feel free to check out Digital Ocean's dead-simple one-click application Ghost droplet.

**Also note:** A slightly outdated tutorial on this topic exists [here](https://www.digitalocean.com/community/tutorials/how-to-create-a-blog-with-ghost-and-nginx-on-ubuntu-14-04)  from which I sourced most of what I cover below.

###Things you'll need

- Digital Ocean account and $50 of free hosting credit (see below)
	- Ubuntu 14.04 Droplet
	- Registered domain name pointed to the IP address of you droplet
	- A non-root user with sudo privileges

First of all, if you haven't already, go sign up for the [Github Student Developer Pack](https://education.github.com/pack). All you need to do is provide your student email (`[uid]@student.42.us.org` for US, `[uid]@student.42.fr` for FR). This pack gives you a wealth of programming goodies, but our main interest is in the $50 of Digital Ocean hosting credit, that's about 10 months of _free_ hosting!

###Setting up your server
Once you've got yourself registered with Digital Ocean and you've got your $50 of credit burning a hole in your virtual pocket, let's go ahead and spin up your first droplet.

<center><img src="https://drive.google.com/uc?export=view&id=0B0ly51VynR0nTVdiRVlCNkhtWDA" alt="recommended options" width=50%></src></center>

_<center>I recommend the above settings as you can always upgrade later.</center>_

Select your version of Ubuntu and the size of your droplet, then select the datacenter region. I'm sure this goes without saying but, closer is better. I would also recommend taking a few moments to set up SSH keys with your droplet and Digital Ocean. Finally, give your droplet an identifying name.

At this point if you have a domain name, you may want to set up a host name with Digital Ocean. To do so, follow this [handy tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean).

Now begins the fun part. Open up your trusty terminal and start a ssh session with your droplet's IP address using the following command

	$ ssh root@[IP address or domain here]

You will be prompted to change your root password since it's your first time logging in but once you do, you're in.
####A brief word about root
Now, as you may or may not know, it's bad practice to operate for extended periods of time as root. This is because by nature root has elevated privileges that can cause irreversible damage. While you may know what you're doing, it's always recommended to make a day-to-day user with limited privileges on any system you manage. So let's do that right now.

####Create a new user
While you're still logged in as root, add a new user that you'll use for your day-to-day maintenance.

	$ adduser [desired username here]

Go ahead and fill out the requested information for your new user (or don't) and choose a password. So now that you have your normal ol' user, let's bump up it's privileges so we don't have to switch to root any time we want to do anything potentially destructive. While we're still root, run the following command to add our new user to the sudo group.

	$ gpasswd -a [username here] sudo

So that's the basics of setting up your new server, there are several other things you can look into like fully configuring public key authentication for more secure logins or setting up a firewall. But I'll let you explore that on your own.

###Time to get spooky
It's now time to install and configure your Ghost server! Assuming you're still logged in as root, let's switch over to our sudo user using `su`.

	$ su - [username here]

Make sure your system is updated.

	$ sudo apt-get update

Ghost requires Node v4.x LTS but at the time of writing, simply installing Node via normal methods will attempt to install Node v7.3.0. How do we get around this? Well, NodeSource maintains what is called a PPA (personal package archive) which will allow you to install a version of Node not currently provided by the Ubuntu repositories. So let's get that set up.

First, curl the bash-script and run it as root to grant you access to Node v4.x LTS, then run the normal installation command.

 	$ curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
	$ sudo apt-get install nodejs

######If you prefer, you may use nvm for this operation but seeing as though we only need this version of node I chose to go with this method

Ensure the correct version of Node is installed.

	$ node -v

If the output reads similar to `v.4.x` then we're good! Our next dependency is npm, let's get that installed.

	$ sudo apt-get install npm

Time to move on to the actual installation of Ghost!

First let's get our file-structure set up and then download the latest stable version (at the time of writing) of Ghost from their Github repository. Please check https://github.com/TryGhost/Ghost/releases/ for the latest version and substitute the URL used below.

	$ sudo mkdir -P /var/www/
	$ cd /var/www/
	$ curl -LO https://github.com/TryGhost/Ghost/releases/download/0.11.3/Ghost-0.11.3.zip
	$ unzip Ghost-0.11.3.zip -d ghost && rm -f Ghost-0.11.3.zip && cd ghost

Now that Ghost is downloaded, our next step is to install Ghost and all of it's dependencies using npm.

	$ npm install --production

If the command completes without errors or timeouts (warnings are fine) then that's it! Ghost is installed and ready to be configured!

###Configuring Ghost
Ghost requires a `config.js` file in the root directory for runtime, as luck has it, Ghost comes with a `config.example.js` ready to be copied. So let's do that.

	$ sudo cp config.example.js config.js

You can set a lot of important details in the config file, but our main focus is in the production section, specifically the line with `url` in it. The url variable must be set to your server's domain if you've set that up, or your server's IP. Use your favorite command-line text editor to make your edit.

```js
config = {
    // ### Production
    // When running Ghost in the wild, use the production environment
    // Configure your URL and mail settings here
    production: {
        url: '[insert domain or IP here]',
        mail: {
            // Your mail settings
        },
        database: {
            client: 'sqlite3',
            connection: {
                filename: path.join(__dirname, '/content/data/ghost.db')
            },
            debug: false
        },

        server: {
            // Host to be passed to node's `net.Server#listen()`
            host: '127.0.0.1',
            // Port to be passed to node's `net.Server#listen()`, for iisnode s$
            port: '2368'
        }
    },
```

Save and exit.

And now for the moment we've been waiting for... let's start it up and hope nothing explodes!

	sudo npm start --production

You should see output similar to the following...

```
> ghost@0.11.3 start /Users/chrisrenfrow/Code/web/chrisrenfrowdotme
> node index

Migrations: Creating tables...
Migrations: Creating table: posts
Migrations: Creating table: users
Migrations: Creating table: roles

[...]

Ghost is running in production...
Your blog is now available on [server IP or domain]
```

You should be able to access your server through `http://your_domain.name:2368` or `http://your_server_ip:2368`!

###But wait, that's not all!
As I'm sure you've already noticed, you can't access your site without including port 2368 in the URL. That's because we need to set up Nginx, a tool that will allow connections on port 80 to connect to the port that Ghost is running on. Let's get it installed.

	$ sudo apt-get install nginx

Remove the default file in `/etc/nginx/sites-enabled/default`

	$ sudo rm /etc/nginx/sites-enabled/default

Now we'll create our file at `/etc/nginx/sites-available/` to allow for nginx to handle our requests on port 80.

	$ sudo touch /etc/nginx/sites-available/ghost

Use your command-line text editor to paste the following code into the file, substituting the server_name variable with your server's domain or IP address.

```
server {
    listen 80;
    server_name your_domain.tld;
    location / {
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   Host      $http_host;
        proxy_pass         http://127.0.0.1:2368;
    }
}
```

Now we'll symlink the configuration file in sites-enabled so edits will be in sync.

	$ sudo ln -s /etc/nginx/sites-available/ghost /etc/nginx/sites-enabled/ghost

Finally, restart Nginx with the following command.

	$ sudo service nginx restart

### Almost there...

Congratulations! You've almost fully configured your blog by hand and have hopefully learned at least a few new things. At this point I would recommend looking into creating another user without elevated privileges that has access to the necessary files to run your blog exclusively. This is to keep your system safe should Ghost become compromised. But I won't go over that in this tutorial.

You may have noticed that logging out of your ssh session kills your site. This is because the npm process that runs your site is attached to your session. To fix this we're going to use a very easy tool called forever. Logged into your non-root user, install forever like so.

	$ sudo npm install -g forever

Set the environment variable NODE_ENV to production and while inside the root ghost directory, start ghost as a detached process with forever.

	$ export NODE_ENV=production
	$ forever start index.js

That's it! You should be able to access your site after exiting the session at your domain or the IP address of your server!

###Where to go from here
Writing posts and editing your blog's preferences is as easy as navigating to `[your domain or IP]/ghost/` and creating a new local account to claim ownership of the blog. If you want to edit the default theme for Ghost or any theme, just navigate to `/var/www/ghost/content/themes/[name of theme]]/` and edit away! Remember additional themes can be found [here](http://marketplace.ghost.org/themes).

If you have any questions or you suspect I've made an error please don't hesitate to contact me at mail@chrisrenfrow.me or slack @crenfrow
