# Install Nodejs

author: Piyush Makhija ([piyushmakhija](https://github.com/piyushmakhija))

collaborators:

* Nick Rameau ([r4meau](https://github.com/r4meau))

Installing Nodejs for the current file user.

# Steps:

1. First, make sure you have [brew installed](https://github.com/all-hack/42moonlight/blob/master/resources/install_brew_fileuser.md)
2. Make sure your brew is the latest version

        brew update
3. Install nvm

        brew install nvm
4. Create an `.nvm` folder in your home directory

        mkdir ~/.nvm
5. Link your nvm directory

        echo 'export NVM_DIR="$HOME/.nvm"\n. "/Volumes/Storage/goinfre/homebrew/opt/nvm/nvm.sh"' >> ~/.zshrc
6. Refresh your .zshrc
        
        source ~/.zshrc
7. Install Nodejs. At the time of writing, 6.9.2 is the latest stable version. Feel free to check the one you need on the [official website](https://nodejs.org)

        nvm install 6.9.2
8. Upgrade npm to the latest version

        npm install -g npm@latest
        
9. Check if everything is fine

        node -v
        npm -v
