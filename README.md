# Debian Linux - Notes

## Install Common Utilities and Applications
- Most applications below require a desktop environment installed such as GNOME, LXDE etc.
- If running later versions of Debian or Ubuntu (Ubuntu 20.04+) packages should be in included package repositories and should not require additional apt sources.
```terminal
# Network and system terminal CLI tools
sudo apt-get -y install net-tools curl wget nmap htop

# Development and version control tools
sudo apt-get -y install git

# Add kernel and module build tools
sudo apt-get -y install build-essential dkms linux-headers-$(uname -r)

# Text editor tools
sudo apt-get -y install vim

# Backup tools
sudo apt-get -y install timeshift

# Privacy and system management tools
sudo apt-get -y install bleachbit
sudo apt-get -y install stacer

# File archive compression tools
sudo apt-get -y install rar unrar

# Image editor tools
sudo apt-get -y install gimp
sudo apt-get -y install inkscape

# Screenshot Screen Capture and Recording tools
sudo apt-get -y install flameshot
sudo apt-get -y install kazam

# Audio file tools
sudo apt-get -y install audacity

# Add video player, tools and codecs
sudo apt-get -y install vlc
sudo apt-get -y install handbrake
sudo apt-get -y install libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-vaapi

# Add additional fonts
sudo apt-get -y install ttf-mscorefonts-installer
sudo apt-get -y install fonts-crosextra-carlito fonts-crosextra-caladea
sudo apt-get -y install fonts-firacode -y
```

## GNOME Specific
```terminal
sudo apt-get -y install gnome-tweaks
```

## Simple UFW firewall with GUI management
```terminal
sudo apt-get -y install ufw gufw
sudo ufw enable
```

## Laptop Specific - Power Management
```terminal
sudo apt-get -y install tlp-get -y
```

## Ubuntu Specific
```terminal
sudo apt-get -y install ubuntu-restricted-extras
```

## NVidia Graphics Specific
```terminal
sudo apt-get -y install nvidia-detect
sudo nvidia-detect
sudo apt-get -y install nvidia-driver
```

## File manager show dotfiles or hidden files
Shortcut `ctrl-h`

## Debian microcode
- Open Synaptic Package Manager
- Search "microcode"
- Install appropriate microsoft AMD or Intel

## Debian enable snap and flatpak support
- Open "Software" application
- Search in Software application for "software"
- Click "Software"
- Scroll down to Add-ons and select Flatpak and Snap Support

## Debian Swappiness
### Check current (default 60)
`cat /proc/sys/vm/swappiness`
### Modify by editting the /etc/sysctl.conf and add line below and reboot.
`vm.swappiness=10`

## Debian increase boot speed edit grub loader /etc/default/grub and modify timeout from 5 to 0
`GRUB_TIMEOUT=0`
### Update GRUB and reboot to test
`sudo update-grub`

## Chrome Extensions
### https://chrome.google.com/webstore/category/extensions
- GNOME Shell Integration

## GNOME Extensions
### https://extensions.gnome.org
- User Themes
- TopIcons Plus

## Create home directories
```bash
mkdir ~/.themes
mkdir ~/.icons
```

### Debian Distro Minimal Install Notes

When prompted unselect all desktop and package options *(use mouse with graphical install or spacebar to unselect normal install)*. The base install should boot to a terminal as no X Window desktop environment should have been selected.

### `sudo` command not found and/or default user not in the sudo group

The mimimal install may mean the `sudo` command may not be installed.
The following commands install the sudo package and adds the default (logged in user) to the sudo group.
```terminal
su -c 'apt-get update && apt-get -y install sudo && \
/sbin/usermod -aG sudo $USER && \
exec su -l $USER'
```
To undo the above (remove user from group sudo and remove package sudo) run the following:  
```terminal
su -c 'gpasswd -d $USER sudo && \
apt-get -y remove sudo && exec su -l $USER'
```

### Updating Debian
```terminal
sudo apt-get update && sudo apt-get -y dist-upgrade
```

### Debian base install - ifconfig, shutdown, reboot, halt and other commands not working

It may be that `/sbin` is missing from the user $PATH variable  
To check current path variables run:
- `echo $PATH` or `env | grep PATH`

Simply edit the `/etc/profile` file and append `/sbin` to the second PATH variable.
- Using nano `sudo nano /etc/profile`
- Using vi `sudo vi /etc/profile`  

*You may need to logout and back in to initialize the new PATH variables.*

Alternatively use the direct path

```terminal
sudo /sbin/reboot
sudo /sbin/halt
sudo /sbin/shutdown
```

Alternatively use the systemctl commands as follows:

```terminal
sudo systemctl reboot
sudo systemctl halt
sudo systemctl poweroff
```

### Install gnome Desktop Environment (core only, no games, office apps etc.)

```terminal
sudo apt-get -y install gnome-core
```

### Start in graphical environment

```terminal
sudo systemctl set-default graphical.target
```

### Start in terminal/console environment

```terminal
sudo systemctl set-default multi-user.target
```

## SSH Related Guides

### Install SSH server (Debian base install)

```terminal
sudo apt-get -y install openssh-server
```

### Backup and Regenerate SSH Server Keys (Debian/Kali)

```terminal
sudo mkdir /etc/ssh/backup_keys && \
sudo mv /etc/ssh/ssh_host_* /etc/ssh/backup_keys && \
sudo dpkg-reconfigure openssh-server
```  
---  

### Check listing ports on host

```terminal
netstat -tulpn
```
Command switches [t] TCP, [u] UDP, [l] listening, [p] program, [n] numeric  

### Disable IPV6 on all interfaces
```terminal
sudo nano /etc/sysctl.conf
```
Add the following line to ```sysctl.conf```:  
`net.ipv6.conf.all.disable_ipv6 = 1`  
*Change all to interface adapter name for individual interfaces*  
Run ```sudo sysctl -p``` to execute changes

### Missing firmware (Debian base install)
Check ```/etc/apt/sources.list``` contains ```contrib non-free``` otherwise edit the file append to end of each deb source line and run the following commands.

If `sources.list` is missing contrib non-free:  
```terminal
sudo sed -r -i 's/^deb(.*)$/deb\1 contrib non-free/g' /etc/apt/sources.list
```  

```terminal
sudo apt-get update && sudo apt-get -y install firmware-misc-nonfree
```

If wifi drivers such those in some Intel NUCs are missing try...

```terminal
sudo apt-get -y install firmware-iwlwifi
```
