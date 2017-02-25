# Install Brew for File User
author: Ian Murawski ([helloitsian](https://www.github.com/helloitsian))

collaborators: Nick Rameau ([r4meau](https://github.com/r4meau))

Installing Brew Package Manager for the current file user.

## Steps

```BASH
git clone https://github.com/Homebrew/homebrew.git /Volumes/Storage/goinfre/$(whoami)/homebrew
echo 'alias brew="/Volumes/Storage/goinfre/$(whoami)/homebrew/bin/brew"' >> ~/.zshrc
source ~/.zshrc
brew update
```
