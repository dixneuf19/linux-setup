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

```bash
sudo dnf install -y htop curl wget git tig jq unzip fzf vim
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

I mainly use Gnome, I have some extensions which I found useful...
In decreasing importance :
* **Tweaks**: useful to manage other extensions
* [**Window List**](https://extensions.gnome.org/extension/602/window-list/): display the windows list at the bottom of the screen. Yep I used to be on Windows...
* **Places status indicator**: quick access to folders
* **System-monitor**: I like to have a quick access to my CPU, RAM and Network use
* **Freon**: used to check for my CPU temperature

How to install these extensions ? I prefer to use the CLI instead of the *horrible* web Gnome extensions interface...

**TODO**: add the CLI snippets to install these extensions

```bash
sudo dnf install gnome-tweaks

```

I may add a gnome config files to save all my configs, but there isn't much to tweak.

At this point I'll configure my shell...

## Configure the dev environnements

#### Python

virtualenv & mkvirtualenv

#### Javascript

nvm (JS)

#### Java

sdkman (java)

#### Golang

go

## Graphical applications

### Minimum (for me)

#### Web browser 

Web browser (I try to stop using chrome, so Firefox here we go)
I should take a look at <https://restoreprivacy.com/firefox-privacy/>

#### Others
vlc
telegram
spotify (snap)
a good text/md editor ?

### Development

vscode
intellij (snap)
snap

### Others

calibre
anki


