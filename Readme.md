# After install Ubuntu
* Get google-chrome & install vimium plugin
* Get dropbox
* Get mendeley

## Install some stuff
**Ubuntu restricted extras**
`sudo apt install ubuntu-restricted-extras`

**Compresion**
`sudo apt-get install rar unace p7zip p7zip-full p7zip-rar unrar lzip lhasa arj sharutils mpack lzma lzop cabextract`

**Productividad**
`sudo apt-get install zsh git-core python3-pip vim-gnome i3 curl libncurses-dev python-dev build-essential cmake libfreetype6-dev feh libcurl4-gnutls-dev pandoc tex-live`

**R & R-Studio**
```bash
sudo echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" | sudo tee -a /etc/apt/sources.list
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
sudo apt-get update
sudo apt-get install r-base r-base-dev

sudo apt-get install gdebi-core
wget https://download1.rstudio.org/rstudio-0.99.896-amd64.deb
sudo gdebi -n rstudio-0.99.896-amd64.deb
```

**oh my zsh**
`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

**Anaconda y Jupyter**
- Download anaconda https://www.continuum.io/downloads
- Install anaconda
`conda install jupyter numpy pandas biopython matplotlib scipy`

**Juyter bash kernel**
```bash
pip install bash_kernel
python -m bash_kernel.install
```

**Jupyter R kernel**
```bash
sudo add-apt-repository ppa:chronitis/jupyter
sudo apt-get update
sudo apt-get install irkernel
```

**Jupyter Perl kernel**
```bash
sudo cpan
install Devel::IPerl
```

**Jupyter themes** --> https://github.com/dunovank/jupyter-themes
`pip install git+https://github.com/dunovank/jupyter-themes.git`

# Config's

## i3

#### Requirements

1. Install i3
```bash
sudo echo "deb http://debian.sur5r.net/i3/ $(lsb_release -c -s) universe" >> /etc/apt/sources.list
sudo apt-get update
sudo apt-get --allow-unauthenticated install sur5r-keyring
sudo apt-get update
sudo apt-get install i3
```

* Get rid of Nautilus desktop. (If this doesn't work, install dconf-tools and maybe libdconf1 and change that option manually from the dconf-editor.)

`gsettings set org.gnome.desktop.background show-desktop-icons false`

* Fonts for airline/powerline:
```bash
git clone https://github.com/powerline/fonts.git
./install.sh
``` 

* Update the system’s font cache.
`sudo fc-cache -f -v`

2. Install rofi
`sudo apt-get install rofi`

#### Configure
* http://hndr.me/blog/making-my-new-linux-less-ugly/
* http://blog.tunnelshade.in/2014/05/making-i3-beautiful.html
* https://github.com/giacomos/i3wm-config/blob/master/config_work_laptop
