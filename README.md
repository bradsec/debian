# Debian and Ubuntu Essentials
## File manager show dotfiles or hidden files
Shortcut `ctrl-h`

## Install common utilities and apps
```bash
sudo apt install gnome-tweaks -y
sudo apt install vlc -y
sudo apt install handbrake -y
sudo apt install audacity -y
sudo apt install gimp -y
sudo apt install timeshift -y
sudo apt install bleachbit -y
sudo apt install rar unrar -y
sudo apt install git
```

## Ubuntu Specific
```bash
sudo apt install ubuntu-restricted-extras -y
```

## Debian microcode
- Open Synaptic Package Manager
- Search "microcode"
- Install appropriate microsoft AMD or Intel

## Debian enable snap and flatpak support
- Open "Software" application
- Search in Software application for "software"
- Click "Software"
- Scroll down to Add-ons and select Flatpak and Snap Support

## Debian swappiness
### Check current (default 60)
`cat /proc/sys/vm/swappiness`
### Modify by editting the /etc/sysctl.conf and add line below and reboot.
`vm.swappiness=10`

## Debian increase boot speed edit grub loader /etc/default/grub and modify timeout from 5 to 0
`GRUB_TIMEOUT=0`
### Update GRUB and reboot to test
`sudo update-grub`

## Debian
```bash
sudo apt install build-essential dkms linux-headers-$(uname -r)
sudo apt install ttf-mscorefonts-installer -y
sudo apt install fonts-crosextra-carlito fonts-crosextra-caladea -y
sudo apt install fonts-firacode -y
sudo apt install build-essential dkms linux-headers-$(uname -r) -y
sudo apt install libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-vaapi -y
```

## Laptop Power Management
```bash
sudo apt install tlp -y
```

## Enable firewall
```bash
sudo apt install ufw -y
sudo ufw enable
sudo apt install gufw
```

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

## NVIDIA Graphics Driver
```bash
sudo apt install nvidia-detect -y
sudo nvidia-detect
sudo apt install nvidia-driver -y
```

## NOTES
### Debian Distro Minimal Install

When prompted unselect all desktop and package options *(use mouse with graphical install or spacebar to unselect normal install)*. The base install should boot to a terminal as no X Window desktop environment should have been selected.

### `sudo` command not found and/or default user not in the sudo group

The mimimal install may mean the `sudo` command may not be installed.
The following commands install the sudo package and adds the default (logged in user) to the sudo group.
```console
su -c 'apt-get update && apt-get -y install sudo && \
/sbin/usermod -aG sudo $USER && \
exec su -l $USER'
```
To undo the above (remove user from group sudo and remove package sudo) run the following:  
```console
su -c 'gpasswd -d $USER sudo && \
apt-get -y remove sudo && exec su -l $USER'
```

### Updating Debian
```console
sudo apt-get update && sudo apt-get -y dist-upgrade
```
### Updating Kali Linux
```console
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

```console
sudo /sbin/reboot
sudo /sbin/halt
sudo /sbin/shutdown
```

Alternatively use the systemctl commands as follows:

```console
sudo systemctl reboot
sudo systemctl halt
sudo systemctl poweroff
```

### Install gnome Desktop Environment (core only, no games, office apps etc.)

```console
sudo apt-get -y install gnome-core
```

### Start in graphical environment

```console
sudo systemctl set-default graphical.target
```

### Start in terminal/console environment

```console
sudo systemctl set-default multi-user.target
```

## SSH Related Guides

### Install SSH server (Debian base install)

```console
sudo apt-get -y install openssh-server
```

### Backup and Regenerate SSH Server Keys (Debian/Kali)

```console
sudo mkdir /etc/ssh/backup_keys && \
sudo mv /etc/ssh/ssh_host_* /etc/ssh/backup_keys && \
sudo dpkg-reconfigure openssh-server
```  
---  

### Check listing ports on host

```console
netstat -tulpn
```
Command switches [t] TCP, [u] UDP, [l] listening, [p] program, [n] numeric  

### Install packages
```console
sudo apt-get -y install net-tools curl wget nmap htop
```

### Disable IPV6 on all interfaces
```console
sudo nano /etc/sysctl.conf
```
Add the following line to ```sysctl.conf```:  
`net.ipv6.conf.all.disable_ipv6 = 1`  
*Change all to interface adapter name for individual interfaces*  
Run ```sudo sysctl -p``` to execute changes

### Missing firmware (Debian base install)
Check ```/etc/apt/sources.list``` contains ```contrib non-free``` otherwise edit the file append to end of each deb source line and run the following commands.

If `sources.list` is missing contrib non-free:  
```console
sudo sed -r -i 's/^deb(.*)$/deb\1 contrib non-free/g' /etc/apt/sources.list
```  

```console
sudo apt-get update && sudo apt-get -y install firmware-misc-nonfree
```

If wifi drivers such those in some Intel NUCs are missing try...

```console
sudo apt-get -y install firmware-iwlwifi
```
