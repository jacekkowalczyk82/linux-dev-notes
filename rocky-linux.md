
# My how-to about Rocky Linux 

* https://www.christitus.com/rocky-linux/

## Vagrant with rocky linux 

vagrant init rockylinux/8 \
  --box-version 4.0.0
  
  

sudo dnf upgrade -y
sudo dnf install --nogpgcheck https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install --nogpgcheck https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-8.noarch.rpm https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-8.noarch.rpm
sudo dnf config-manager --set-enabled PowerTools

Note: If you can’t install PowerTools with config-manager, you can manually enable it through the repo file in /etc/yum.repos.d

sudo dnf update -y
sudo dnf install xorg-x11-server-Xorg xorg-x11-xauth xorg-x11-apps -y
sudo dnf install plasma-desktop kscreen sddm kde-gtk-config dolphin konsole kate -y

udo systemctl set-default graphical.target
sudo systemctl enable sddm

sudo dnf groupinstall "Development Tools"
sudo dnf install cmake gcc-c++ libX11-devel libXext-devel qt5-qtx11extras-devel qt5-qtbase-devel qt5-qtsvg-devel qt5-qttools-devel kf5-kwindowsystem-devel make procps-ng curl file git

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Note: Installer messed up and put it in /home/linuxbrew/.linuxbrew I moved this to my home folder and linked the brew executable to ~/bin

Add Homebrew to the end bash or zsh rc file

eval $(~/.linuxbrew/bin/brew shellenv)


sudo rpm --import https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg
printf "[gitlab.com_paulcarroty_vscodium_repo]\nname=gitlab.com_paulcarroty_vscodium_repo\nbaseurl=https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/rpms/\nenabled=1\ngpgcheck=1\nrepo_gpgcheck=1\ngpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg" |sudo tee -a /etc/yum.repos.d/vscodium.repo
sudo dnf install codium


sudo dnf install vim nano curl mc git neofetch tmux htop 
   
sudo dnf install xorg-x11-server-Xorg xorg-x11-xauth  -y
sudo dnf install geany -y 



sudo systemctl disable sddm
sudo systemctl set-default multi-user.target
sudo dnf remove plasma-desktop kscreen sddm kde-gtk-config dolphin konsole kate -y



sudo dnf groupinstall "Xfce"
sudo dnf install thunar xfce4-terminal mousepad  f35-backgrounds-xfce -y

echo "exec startxfce4" > ~/.xinitrc 

#echo "exec /usr/bin/xfce4-session" >>  ~/.xinitrc


* https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-install-virtualbox-on-centos-8-rhel-8.html

sudo dnf config-manager --add-repo=https://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo
sudo rpm --import https://www.virtualbox.org/download/oracle_vbox.asc
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y

sudo dnf install binutils kernel-devel kernel-headers libgomp make patch gcc glibc-headers glibc-devel dkms -y
sudo dnf update kernel-*

sudo dnf install gcc make perl kernel-devel kernel-headers bzip2 dkms
sudo dnf install -y wget

sudo dnf update kernel-*

sudo dnf search VirtualBox
sudo dnf install VirtualBox-6.1 -y

sudo usermod -aG vboxusers jacek 

systemctl status vboxdrv
systemctl start vboxdrv





