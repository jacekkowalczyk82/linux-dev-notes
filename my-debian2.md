# project my-debian 2 LDD scripts for debian server 

```
wget https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/amd64/iso-cd/firmware-11.2.0-amd64-netinst.iso


```


su - 
apt install sudo 
usermod -aG sudo jacek

sudo apt update
sudo apt -y install tmux git mc nano vim curl wget htop

sudo apt install bspwm xorg nitrogen variety stterm dmenu polybar rxvt-unicode ssh rsync make gcc clang

mkdir bin/


touch .xinitrc
nano .xinitrc

Add the following statement in this file

exec bspwm


mkdir .config/bspwm
nano .config/bspwm/bspwmrc



#! /bin/sh

pgrep -x sxhkd > /dev/null || sxhkd &

$HOME/.config/polybar/launch.sh 

bspc monitor -d 1 2 3 4 5 6 7 8 9             

bspc config border_width         2
bspc config window_gap          12

bspc config split_ratio          0.52
bspc config borderless_monocle   true
bspc config gapless_monocle      true

bspc rule -a Gimp desktop='^8' state=floating follow=on
bspc rule -a Chromium desktop='^2'
bspc rule -a mplayer2 state=floating
bspc rule -a Kupfer.py focus=on
bspc rule -a Screenkey manage=off


https://b.anmsh.net/fresh-debian-testing-setup-with-bspwm/



mkdir -p ~/.config/{bspwm,sxhkd,polybar}
sudo cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/bspwmrc_default.txt
sudo cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/sxhkdrc
sudo cp /usr/share/doc/polybar/config ~/.config/polybar/config

sudo chown -R jacek:jacek /home/jacek/.config/

chmod +x ~/.config/bspwm/bspwmrc
chmod +x ~/.config/sxhkd/sxhkdrc


nano ~/.config/sxhkd/sxhkdrc


alt + Return
        st




nano ~/.config/polybar/launch.sh


killall -q polybar

while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done

polybar bar1 2>&1 | tee -a /tmp/polybar-bar1.log & disown


chmod +x ~/.config/polybar/launch.sh


nano ~/.config/polybar/config

[colors]
background = #222
background-alt = #444
foreground = #dfdfdf
foreground-alt = #999
primary = #ffb52a
secondary = #e60053
alert = #bd2c40

[bar/bar1]
width = 100%
height = 27
radius = 3.0
fixed-center = false
bottom = true
background = ${colors.background}
foreground = ${colors.foreground}

font-0 = fixed:pixelsize=12;1

modules-left = bspwm
modules-center =
modules-right = alsa memory cpu eth wlan temperature date

