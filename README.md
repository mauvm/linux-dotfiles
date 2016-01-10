# Dotfiles

My Linux configuration.

## How to use

```bash
git clone --recursive https://github.com/mauvm/linux-dotfiles.git ~/.dotfiles
~/.dotfiles/install

# > Follow https://help.ubuntu.com/community/MacBookPro11-1/Saucy

# Apt Get
apt-add-repository ppa:detly/mactel-utils
echo "deb http://debian.sur5r.net/i3/ $(lsb_release -c -s) universe" >> /etc/apt/sources.list
apt-get update
apt-get --allow-unauthenticated install sur5r-keyring
apt-get update
apt-get install -y \
	bcmlwl-kernel-source x11-xserver-utils xinit xlm-sensors hddtemp \
	i3 i3status fonts-droid conky-all \
	xclip xdotool xbacklight network-manager \
	xterm git-core silversearcher-ag htop jq \
	firefox flashplugin-installer zathura filezilla transmission-daemon vlc \
	zip unzip wget curl httpie lftp

# Install Docker
apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" >> /etc/apt/sources.list.d/docker.list
apt-get update
apt-get purge lxc-docker
apt-get install -y linux-image-extra-$(uname -r) docker-engine

curl -L https://github.com/docker/compose/releases/download/1.5.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

# Dotfiles
git clone https://github.com/mauvm/linux-dotfiles ~/.dotfiles
~/.dotfiles/install

# Xsession
vim /etc/X11/Xsession
# > Change ERRFILE=/var/log/xsession-errors

# Git
git config --global user.email "maurits@nerdieworks.nl"
git config --global user.name "Maurits van Mastrigt"

# Fix typing quotes and special characters
dpkg-configure keyboard-configuration
# > Select US (Macintosh)
# > Select RightAltr as alt and compose keys

# Add wireless
# > https://docs.fedoraproject.org/en-US/Fedora/20/html/Networking_Guide/sec-Connecting_to_a_Network_Using_nmcli.html

# TODO: Read about: https://faq.i3wm.org/question/239/how-do-i-suspendlockscreen-and-logout/

# Firefox
# > Go to about:config and change layout.css.devPixelsPerPx to 1.3
# > Go to Menu > Sign in to Sync and login

# Dropbox
apt-key adv --keyserver pgp.mit.edu --recv-keys 5044912E
#add-apt-repository "deb http://linux.dropbox.com/ubuntu $(lsb_release -sc) main"
apt-get update
apt-get install -y libxlst1.1 dropbox
dropbox start -i # Not as root
dropbox autostart y

# Telegram
apt-add-repository ppa:noobslab/telegram
apt-get update
apt-get install -y telegram-desktop

# Hub
add-apt-repository ppa:cpick/hub
apt-get update
apt-get install -y hub

# NodeJS
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
apt-get install -y nodejs

# Slack
apt-add-repository ppa:rael-gc/scudcloud
apt-get update
apt-get install -y scudcloud

# LastPass
# > Install LastPass Firefox extension

# VirtualBox
apt-get install -y virtualbox
cd ~/Downloads
FILE=Oracle_VM_VirtualBox_Extension_Pack-4.3.10-93012.vbox-extpack
curl -O http://download.virtualbox.org/virtualbox/4.3.10/$FILE
virtualbox $(pwd)/$FILE

# Fix sound (mine was not working)
apt-get install -y alsa-base pulseaudio
aptitude --purge reinstall linux-sound-base alsa-base alsa-utils linux-image-`uname -r` linux-ubuntu-modules-`uname -r` libasound2
```

# Node tools
npm install --global grunt-cli bower

# Transmission
usermod -a -G debian-transmission user

# Google Chrome
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
apt-get update
apt-get install google-chrome-stable
# > Add Google Cast extension: https://chrome.google.com/webstore/detail/google-cast/boadgeojelhgndaghljhdicfkmllpafd/
