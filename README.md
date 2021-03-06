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
	i3 i3status fonts-droid conky-all redshift \
	xclip xdotool xbacklight network-manager \
	xterm git-core silversearcher-ag htop jq g++ \
	firefox flashplugin-installer zathura filezilla transmission-daemon vlc \
	zip unzip unrar wget curl httpie lftp scrot nmap

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
# > Select Generic 105-key (Intl) PC
# > Select English (US)
# > Select English (Macintosh)
# > Select Left Alt
# > Select No compose key
# > Select No

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

# VPN
apt-get install -y openvpn
# > Follow https://www.gaggl.com/2013/04/openvpn-forward-all-client-traffic-through-tunnel-using-ufw/
# > Also add wlan0 line in /etc/ufw/before.rules

# LastPass CLI
apt-get install -y openssl libcurl4-openssl-dev libxml2 libssl-dev libxml2-dev pinentry-curses xcli
cd /tmp/
git clone https://github.com/lastpass/lastpass-cli
cd lastpass-cli/
make
make install
lpass login <username>

# Tor browser
add-apt-repository ppa:webupd8team/tor-browser
apt-get update
apt-get install -y tor-browser

# Keybase CLI
curl -O https://dist.keybase.io/linux/deb/keybase-latest-amd64.deb
dpkg -i keybase-latest-amd64.deb
rm keybase-latest-amd64.deb

# Fix slow Firefox
# > Firefox > Preferences > Advanced > Network > Check 'Override automatic cache management'
#   and limit cache to 1024mb
# > Add 'APT::Cache-Limit "100000000";' to /etc/apt/apt.conf.d/70debconf
apt-get clean
apt-get update --fix-missing
# > Restart Firefox

# Better notification lib
apt-get remove dunst
apt-get install -y notify-osd notify-osd-icons libnotify-bin

# Fan control
# > Add line "coretemp" to /etc/modules
# > Add line "applesmc" to /etc/modules
cd /usr/local/src
sudo git clone https://github.com/dgraziotin/Fan-Control-Daemon.git
sudo chmod -R 755 /usr/local/src/Fan-Control-Daemon
sudo chown -R $USER:$USER /usr/local/src/Fan-Control-Daemon
cd Fan-Control-Daemon
make
sudo make install
sudo cp mbpfan.upstart /etc/init/mbpfan.conf
sudo start mbpfan

# Mopidy
wget -q -O - https://apt.mopidy.com/mopidy.gpg | apt-key add -
wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/jessie.list
apt-get update
apt-get install -y mopidy mopidy-alsamixer mopidy-soundcloud mopidy-spotify ncmpcpp
vim /etc/init.d/mopidy
# > CONFIG_FILES="/usr/share/mopidy/conf.d:/etc/mopidy/mopidy.conf:/home/mauvm/.config/mopidy/mopidy.conf"

# Docker
# > Add DOCKER_OPTS="-r=false" to /etc/init/docker.conf to disable container auto start

# AVRDude
add-apt-repository ppa:pmjdebruijn/avrdude-release
apt-get update
apt-get install -y avrdude

# VLC codecs
apt-add-repository ppa:strukturag/libde265
apt-get update
apt-get install -y vlc-plugin-libde265

# Skype
dpkg --add-architecture i386
add-apt-repository "deb http://archive.canonical.com/ $(lsb_release -sc) partner"
apt-get update && sudo apt-get install skype
```
