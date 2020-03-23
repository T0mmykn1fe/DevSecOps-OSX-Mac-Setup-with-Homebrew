![alt tag](https://coolestguidesontheplanet.com/wp-content/uploads/2013/12/home-brew-osx-lion-package-manager.png)

# BrewSetup for your DevSecOps Mac

Why spend time downloading pesky installers form the web,
Just run this guy, grab a coffee and come back to a fully setup Mac for DevOps and Security testing 

Approximate setup time is roughly 1-hour 

The best bit about this, its idempotent so run it as many times as you like, it will know whats already installed and not install apps twice.

### Preferences


#### Show Library folder

```shell
chflags nohidden ~/Library
```

### Show hidden files

This can also be done by pressing `command` + `shift` + `.`.

```shell
defaults write com.apple.finder AppleShowAllFiles YES
```

#### Show path bar

```shell
defaults write com.apple.finder ShowPathbar -bool true
```

#### Show status bar

```shell
defaults write com.apple.finder ShowStatusBar -bool true
```

#### Disable unidentified developer warnings

```shell
sudo spctl --master-disable
```

#### Save Screenshots to alternative folder

I usually use this command after installing every application that I need - it keeps Apple applications on the first page and moves the rest to the next pages.

```shell
$ defaults write com.apple.screencapture location ~/Pictures/screenshots
$ killall SystemUIServer
```

#### Set Firmware Password

Setting a firmware password prevents your Mac from starting up from any device other than your startup disk. It may also be set to be required on each boot.

```shell
$ sudo firmwarepasswd -setpasswd
```

### Install Homebrew

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Turn Homebrew Analytics off

Homebrew now defaults to retrieving behavioral analytics tracking. Although anonymized, you may not want to participate in that. To disable the extra network traffic:

```shell
brew analytics off
```

### Run the Brewfile

```shell
brew bundle #In the same dirctory you downloaded the source code into
```

#### Git config

Make sure to replace name and email with your personal details.
```shell
git config --global user.name "yourusername"
git config --global user.email "*.******@***.***"
```

##### Generate key
```shell
ssh-keygen -t ed25519 -a 100
```
##### Copy key
```shell
cat ~/.ssh/id_rsa.pub | pbcopy
```

##### Add to Github

Github SSH key [https://github.com/settings/ssh]

##### Test connection
```shell
ssh -T git@github.com
```

#### ZSH config 
oh my ZSH

```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
Lets change the theme now
You can browse all the “Oh My ZSH” Themes here. To change the Theme, simply change the ZSH_THEME value in ~/.zshrc file from robbyrussell to Avit.
Run the following command to update the config.
```shell
$ source ~/.zhrc
```
Lets add some backround config with Iterm
```shell
Open ITerm2 > Preferences > Profiles > Colors and change the background black color to use 20% gray
```

##### Install Powerline fonts
```shell
$ cd ~/.oh-my-zsh
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
./install.sh
```
change theme to 'agnoster'
```shell
$ open ~/.zshrc
Set ZSH_THEME="agnoster" and save the file
```
##### Set Powerline font
You can set any Powerline patched font you like. All the fonts end with “for Powerline”.
```shell
Open ITerm2 > Preferences > Profiles > Text > Change Font and set it to “Meslo LG DZ for Powerline” font.
```

##### Install iTerm2 “color schemes” (

Download the iTerm2-color-schemes as a zip file and extract it

```shell
git clone https://github.com/mbadolato/iTerm2-Color-Schemes
```

The “Schemes” folder contains all the color scheme files — they end with .itermcolors
```shell
Open iTerm2 > Preferences > Profile > Colors > Color Presets > Import
```
In the import window, navigate to the “Schemes” folder

Select all the files so you can import all the color schemes at once
Simply select whichever color scheme you like.

Select theme "Argonaut"

You should have a terminal that looks like this:
![alt tag](https://user-images.githubusercontent.com/6216224/41241608-5f1678a4-6d6b-11e8-8b0f-cac343716f6c.png)


##### Add Syntax Highlighting

Clone the zsh-syntax-highlighting plugin’s repo and copy it to the “Oh My ZSH” plugins directory.
```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
Activate the plugin in ~/.zshrc by adding `zsh-syntax-highlighting to the Plugins section as shown below.
![alt tag](https://cdn-images-1.medium.com/max/1600/1*1sGebsi0qMQMAvPLo64ARQ.png)
##### Add ZSH-AutoSuggestion Plugin

This plugin auto suggests any of the previous commands. Pretty handy! To select the completion, simply press → key.

Install the plugin
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```
Open ~/.zshrc and add zsh-autosuggestions
![alt tag](https://cdn-images-1.medium.com/max/1600/1*pshPBacVfZgHaKdlG1cajg.png)
##### Add Cowsay, Fortune and Lolcat

```shell
$ brew install cowsay
$ brew install fortune
$ sudo gem install lolcat
```
Then head into your .zshrc file and add the config to the bottom of the file
```shell
Fortune | cowsay -f vader | lolcat
```
![alt tag](https://camo.githubusercontent.com/d27a8eb644869e5ecaee2dc35382afcc5f547adf/68747470733a2f2f7261772e6769746875622e636f6d2f627573796c6f6f702f6c6f6c6361742f6d61737465722f6173732f73637265656e73686f742e706e67)

#### Pimp your NANO!

Why not use the best text editor with syntax highlighting for everything.

Simply take the below scripts, "touch" it into a "nano4upgrade.sh" and run it after a "chmod a+x"

Or just run it from within this repo 

```shell
#!/usr/bin/env bash

# Install Nano (www.nano-editor.org) with syntax highlighting (MacOS)

VERSION="4.8"
NANO_SHORT="nano-$VERSION"
NANO_SRC="$NANO_SHORT.tar.gz"
NANO_URL="https://www.nano-editor.org/dist/v4"
NANO_EXTRA="https://github.com/scopatz/nanorc"

cd ~/
curl -Ok $NANO_URL/$NANO_SRC
tar -zxvf $NANO_SRC

mv $NANO_SHORT .nano && cd .nano/
./configure && make && sudo make install

git clone --depth=1 $NANO_EXTRA syntax_improved
cd ~/ && touch .nanorc

cat > .nanorc <<EOF
include ~/.nano/syntax/*.nanorc
include ~/.nano/syntax_improved/*.nanorc
EOF

rm -vf $NANO_SRC
printf "\nExit terminal and reopen to start using $NANO_SHORT\nTo uninstall it and revert to old:\ncd ~/.nano && sudo make clean uninstall && rm -rf ~/.nano\n"
exit
```

##### Git Cheatsheet

| COMMAND     | DESCRIPTION                                                         |
| ----------- | ------------------------------------------------------------------- |
| `clone`     | Clone a repository into a new directory                             |
| `init`      | Create an empty Git repository or reinitialize an existing one      |
| `add`       | Add file contents to the index                                      |
| `mv`        | Move or rename a file, a directory, or a symlink                    |
| `reset`     | Reset current HEAD to the specified state                           |
| `rm`        | Remove files from the working tree and from the index               |
| `log`       | Show commit logs                                                    |
| `show`      | Show various types of objects                                       |
| `status`    | Show the working tree status                                        |
| `branch`    | List, create, or delete branches                                    |
| `checkout`  | Switch branches or restore working tree files                       |
| `commit`    | Record changes to the repository                                    |
| `diff`      | Show changes between commits, commit and working tree, etc          |
| `merge`     | Join two or more development histories together                     |
| `tag`       | Create, list, delete or verify a tag object signed with GPG         |
| `pull`      | Fetch from and integrate with another repository or a local branch  |
| `push`      | Update remote refs along with associated objects                    |

##### Brew Cheatsheet

![alt tag](https://res.cloudinary.com/practicaldev/image/fetch/s--s5G3V2SE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://www.code2bits.com/wp-content/uploads/cheatsheet-image/cheatsheet-homebrew.png)