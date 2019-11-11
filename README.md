# Reinstall my Linux OS

I do it sometimes for good or bad reasons. These is a guide intended to do it quicker next time. At the end, it should become a script, because [automation is important](https://queue.acm.org/detail.cfm?id=3197520) and it can be done [gradually](https://blog.danslimmon.com/2019/07/15/do-nothing-scripting-the-key-to-gradual-automation/).

The idea :
1. Document the steps: just plain text, a kitchen recipe
2. Create automation equivalents: CLI snippets
3. Create automation: Transform it into a script, *Go* seems to be a good candidate as it is a type-checked and fast language. *Python* is also a safe choice.
4. Self-service and autonomous systems: next step, out of context here

Question: I'd like to keep a *text* documentation, like this markdown, with the script. How should I synchronize both ?

## BIOS Setup

Can be useful, check for the ArchLinux wiki (for my [XPS13](https://wiki.archlinux.org/index.php/Dell_XPS_13_(9370)))

## Install the OS

Fedora Workstation with Gnome is my goto for the moment. Ubuntu is always a safe bet, and Manjaro seems OK too. 

### Disk partitioning

I hade some issues because I was booting the USB-key legacy mode, and the installation kept asking to create a 1MB *GPT* partition or whatever. Be sure to boot in UEFI mode if the current installation is in this mode.

For my dual-boot Fedora 31 install, I used this partitioning scheme :
- a lvm partition `/` as root partition for Fedora
- 1024 MB *boot* partion (*ext4*) mounted on `/boot`
- a ~700 MB *efi* partition mounted on `'/boot/efi`

Advice: If you don't have much place on your only disk (my dual boot XPS13 9370 256G), avoid to separate `/home` and `/` in separated `lvm` partitions.  


## Shell configuration

### CLI tools

Tools that i found useful :
- htop: better top, used to monitor ressources usage
- curl
- wget
- git
- tig: interactive `git status`
- jq: useful to parse json
- unzip: use to extract *zip* archives
- fzf: a command line fuzzy finder
- vim
- httpie

```bash
sudo dnf install -y htop curl wget git tig jq unzip fzf vim httpie
```

### Oh my god, zsh!

*zsh* and *oh-my-zsh* are easy to install and so powerful, a no-brainier for me...

```bash
sudo dnf install -y zsh powerline-fonts bzr
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
If you have your `.zshrc` lying around, you can now `source .zshrc`.

Otherwise here are some tweaks which i found important

#### Shell theme

I kind of like *frisk* theme.

```bash
sed -i 's/ZSH_THEME=".*"/ZSH_THEME="frisk"/' .zshrc
```

I need to tweak a bit the dark color of the terminal after...

#### zsh plugins

I use:
- common-aliases
- git
- docker: autocompletion 
- fzf: better *CTRL + R*

I'll add specific languages tools later.

```bash
plugins=(
  git
  common-aliases
  docker
  fzf
)
```

### Time to git and ssh

I follow the [GitHub tutorial](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

```bash
ssh-keygen -t rsa -b 4096 -C "julen@loudnaround.org"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
sudo dnf install -y xclip
xclip -sel clip < ~/.ssh/id_rsa.pub
```

Then paste this key into the web interface of [GitHub](https://github.com/settings/ssh/new), [GitLab.com](https://gitlab.com/profile/keys), etc...

Don't forget to configure your name and email on git !

```bash
git config --global user.name "Julen Dixneuf"
git config --global user.email "julen@loudnaround.org"
```

## Graphic environment

### Gnome extensions

I mainly use Gnome, I have some extensions which I found useful...
In decreasing importance :
* **Tweaks**: useful to manage other extensions
* [**Window List**](https://extensions.gnome.org/extension/602/window-list/): display the windows list at the bottom of the screen. Yep I used to be on Windows...
* [**Places status indicator**](https://extensions.gnome.org/extension/8/places-status-indicator/): quick access to folders
* [**system-monitor**](https://extensions.gnome.org/extension/120/system-monitor/): I like to have a quick access to my CPU, RAM and Network use
* [**Freon**](https://extensions.gnome.org/extension/841/freon/): used to check for my CPU temperature

How to install these extensions ? I prefer to use the CLI instead of the *horrible* web Gnome extensions interface...

```bash
sudo dnf install gnome-tweaks
```

To install with CLI, I used this [script](https://github.com/cyfrost/install-gnome-extensions). To install it :

```bash
rm -f ./install-gnome-extensions.sh; wget -N -q "https://raw.githubusercontent.com/cyfrost/install-gnome-extensions/master/install-gnome-extensions.sh" -O ./install-gnome-extensions.sh && chmod +x install-gnome-extensions.sh && ./install-gnome-extensions.sh
```

Then you need to supply all the link of the extension in a file, `extensions.txt` for instance

```
https://extensions.gnome.org/extension/602/window-list/
https://extensions.gnome.org/extension/8/places-status-indicator/
https://extensions.gnome.org/extension/120/system-monitor/
https://extensions.gnome.org/extension/841/freon/
```

Then you supply it to the cli.

```bash
./install-gnome-extensions.sh --enable --file links.txt
```

I may add a gnome config files to save all my configs, but there isn't much to tweak anyway.
### Wallpapers

You can install extra fedora backgrounds with

```bash
sudo dnf install f31-backgrounds-extras-gnome.noarch
```

Then you select the `/usr/share/backgrounds/f31/extras/f31-extras.xml` which points at the wallpapers in `gnome-tweaks`.

## Configure the dev environnements

#### Python

See my [gist](https://gist.github.com/dixneuf19/a398c08f00aac24609c3cc44c29af1f0).

#### Javascript

TODO: nvm (JS)

#### Java

[SDKMAN!](https://sdkman.io) is a very handy java JDK manager.

```bash
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk version
```

And you should be good to go ! You can run `sdk install java` and `sdk install gradle`.

You can list all available *java* installations with `sdk list java`, and use a specific one with `sdk use`, or default with `sdk default`.

#### Golang

```
sudo dnf install -y go
mkdir $HOME/go
```

Add in you `.zshrc` :

```bash
#GOPATH
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

## Graphical applications

### Minimum (for me)

#### Web browser 

Web browser (I try to stop using chrome, so Firefox here we go)
I should take a look at <https://restoreprivacy.com/firefox-privacy/>

#### Others

You'll need the RPM fusion repositories :

```bash
sudo dnf install -y https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

- [**VLC**](https://www.videolan.org/vlc/download-fedora.html)

```bash
sudo dnf install -y vlc
```

- [**Telegram**](https://telegram.org/)

```bash
sudo dnf install -y telegram-desktop
```

- [**Snap**](https://snapcraft.io/): the canonical packaging system

```bash
sudo dnf install -y snapd
sudo ln -s /var/lib/snapd/snap /snap
```

- [**Spotify**](https://www.spotify.com/fr/download/linux/)

Using *Snap*

```bash
snap install spotify
```

- [**UberWriter**](https://flathub.org/apps/details/de.wolfvollprecht.UberWriter): an OK Markdown editor. Use the link.

- **Transmission**: a very simple torrent downloader. `sudo dnf install -y transmission`

### Development

#### [**VS Code**](https://code.visualstudio.com/docs/setup/linux)

For almost everything!

Snap installation is simple

```bash
snap install --classic code
```

A few extensions that I found useful :
- Python
- GitLens
- Docker
- Go
- Lorem ipsum


#### [IntelliJ](https://www.jetbrains.com/idea/)

For Java.

```bash
snap install intellij-idea-ultimate --classic
```

If you don't have the ultimate version:

```bash
snap install intellij-idea-community --classic
```

To use your sdk `java` and `gradle` in your project, configure the correct paths in `File | Settings | Build, Execution, Deployment | Build Tools | Gradle` in IntelliJ :
- Gradle : `$HOME/.sdkman/candidates/gradle/current`
- Java : `$HOME/.sdkman/candidates/java/current`


### Others

- **Calibre**: E-book library manager. `sudo dnf install -y calibre`
- **Anki**: Flash card for spaced repetition. `sudo dnf install -y anki`


