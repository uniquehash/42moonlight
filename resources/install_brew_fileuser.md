# Install Brew for File User
author: Ian Murawski ([helloitsian](https://www.github.com/helloitsian))

Installing Brew Package Manager for the current file user.

## Steps

1. `cd ~/goinfre`
2. `git clone https://github.com/Homebrew/homebrew.git`
3. `export PATH=ABSOLUTE_PATH_TO_HOME/goinfre/homebrew/bin:${PATH}`
	* `ABSOLUTE_PATH_TO_HOME` example: `/nfs/2016/i/YOUR_LOGIN/`
4. `brew update`

## Persistent $PATH change
1. open `~/.bash_profile` in text editor
2. add instruction 3 from above to the file
3. save
4. command line: `source $HOME/.bash_profile`
