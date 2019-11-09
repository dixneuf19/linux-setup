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

Advice: If you don't have much place on your only disk (my dual boot XPS13 9370 256G), avoid to separate `/home` and `/` in separated `lvm` partitions.  

## Graphic environment

I mainly use Gnome, I have some extensions which I found useful...
In decreasing importance :
* **Tweaks**: useful to manage other extensions
* **Window List**: display the windows list at the bottom of the screen. Yep I used to be on Windows...
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

## Shell configuration

### Oh my god, zsh!

*zsh* and *oh-my-zsh* are easy to install and so powerful, a no-brainier for me...
zsh

### Time to git and ssh

ssh key with github, gitlab (CS, com, VR)

### CLI tools

fzf
tig
htop

### Configure the dev environnements

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


