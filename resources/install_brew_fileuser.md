# Install Brew for File User
author: Ian Murawski ([helloitsian](https://www.github.com/helloitsian))

collaborators: Nick Rameau ([r4meau](https://github.com/r4meau))

Installing Brew Package Manager for the current file user.

## Steps

```BASH
cd /Volumes/Storage/goinfre/
git clone https://github.com/Homebrew/homebrew.git
echo 'alias brew="/Volumes/Storage/goinfre/homebrew/bin/brew"' >> ~/.zshrc
source ~/.zshrc
brew update
```
