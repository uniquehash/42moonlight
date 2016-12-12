## Install Homebrew at Login

1. Open Automator app

2. Select "Application"

3. Add "Run Shell Script" action

4. Copy & Paste the following to the script:

	```shell
	cd /Volumes/Storage/goinfre/mfernand
	git -C brew pull || git clone git@github.com:Homebrew/brew.git
	source ~/.zshrc
	```

5. (Optional) You can also install apps at startup, for example:

	```shell
	brew install speedtest
	brew install tree
	brew install unrar
	brew install htop
	```

6. Hit "Run" to test

7. Save the Automator script somewhere in your HOME. I have it on my Desktop.

8. Go to System Preference -> Users & Groups -> Login Items

9. Add this app.

10. Test it by logging into a different computer.

Done :)

![Example Script](/resources/images/brew_install_script.png?raw=true "Example Script")
