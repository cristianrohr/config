# After install Ubuntu

* Get google-chrome
* Get dropbox

## Install some stuff
* Ubuntu restricted extras
`sudo apt install ubuntu-restricted-extras`

* Compresion
`sudo apt-get install rar unace p7zip p7zip-full p7zip-rar unrar lzip lhasa arj sharutils mpack lzma lzop cabextract`

* Productividad
`sudo apt-get install zsh git-core python3-pip vim-gnome i3 curl libncurses-dev python-dev build-essential cmake libfreetype6-dev feh`

* oh my zsh
`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

- Install i3 WM
```bash
sudo echo "deb http://debian.sur5r.net/i3/ $(lsb_release -c -s) universe" >> /etc/apt/sources.list
sudo apt-get update
sudo apt-get --allow-unauthenticated install sur5r-keyring
sudo apt-get update
sudo apt-get install i3
```

- Get rid of Nautilus desktop. (If this doesn't work, install dconf-tools and maybe libdconf1 and change that option manually from the dconf-editor.)

`gsettings set org.gnome.desktop.background show-desktop-icons false`

- Fonts for airline/powerline:
```bash
git clone https://github.com/powerline/fonts.git
./install.sh
``` 

Update the system’s font cache.
`sudo fc-cache -f -v`

# Config's

## i3

#### Requirements

Step 1. Install i3
Step 2. Install rofi
`sudo apt-get install rofi`

#### Configure
* http://hndr.me/blog/making-my-new-linux-less-ugly/
* http://blog.tunnelshade.in/2014/05/making-i3-beautiful.html
* https://github.com/giacomos/i3wm-config/blob/master/config_work_laptop
