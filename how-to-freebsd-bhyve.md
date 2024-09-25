# Manuals and notes related to FreeBSD bhyve 

## manual way by https://www.cyberciti.biz/faq/how-to-install-linux-vm-on-freebsd-using-bhyve-and-zfs/

* https://www.cyberciti.biz/faq/how-to-install-linux-vm-on-freebsd-using-bhyve-and-zfs/
* https://www.truenas.com/community/threads/quick-guide-to-install-debian-10-in-bhyve-with-the-serial-console.85424/
* https://forums.freebsd.org/threads/bhyve-and-debian.81602/


```

su - 

ifconfig tap0 create
# sysctl net.link.tap.up_on_open=1
net.link.tap.up_on_open: 0 -> 1
echo "net.link.tap.up_on_open=1" >> /etc/sysctl.conf

kldload vmm
kldload nmdm

vmm_load="YES"
nmdm_load="YES"
if_tap_load="YES"
if_bridge_load="YES"


echo 'vmm_load="YES"' >> /boot/loader.conf
echo 'nmdm_load="YES"' >> /boot/loader.conf
echo 'if_tap_load="YES"' >> /boot/loader.conf
echo 'if_bridge_load="YES"' >> /boot/loader.conf


ifconfig bridge0 create
ifconfig bridge0 addm em0

ifconfig bridge0 addm em0 addm tap0
ifconfig bridge0 up

# add to /etc/rc.conf
# byhve and jail settings bridges 
cloned_interfaces="bridge0 tap0"
ifconfig_bridge0_name="em0bridge"
ifconfig_em0bridge="addm em0 addm tap0 up"


zfs create -V60G -o volmode=dev zroot/debianvm

reboot 

mkdir -p ~/bhyve/debian-11/
cd ~/bhyve/debian-11/
wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.1.0-amd64-netinst.iso

bhyve -c 1 -m 2G -w -H \
-s 0,hostbridge \
-s 3,ahci-cd,/usr/home/jacek/bhyve/debian-11/debian-11.1.0-amd64-netinst.iso \
-s 4,virtio-blk,/dev/zvol/zroot/debianvm \
-s 5,virtio-net,tap0 \
-s 29,fbuf,tcp=0.0.0.0:5900,w=800,h=600,wait \
-s 30,xhci,tablet \
-s 31,lpc -l com1,stdio \
-l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI.fd \
debianvm




sudo pkg install tightvnc

vncviewer IP:5900




mkdir /target/boot/efi/EFI/BOOT/
# copy file - workaround for bhyve grub package #
# Pay attention to destination file bootx64.efi #
cp /target/boot/efi/EFI/debian/grubx64.efi /target/boot/efi/EFI/BOOT/bootx64.efi




bhyvectl --destroy --vm=debianvm

Finally, boot the Debian 10 virtual machine and enjoy the fruits of our labor:
bhyve -c 2 -m 2G -w -H \
-s 0,hostbridge \
-s 4,virtio-blk,/dev/zvol/zroot/debianvm \
-s 5,virtio-net,tap0 \
-s 29,fbuf,tcp=0.0.0.0:5900,w=1024,h=768,wait \
-s 30,xhci,tablet \
-s 31,lpc -l com1,stdio \
-l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI.fd \
debianvm




#!/bin/sh
# Name: startdebianvm
# Purpose: Simple script to start my Debian 10 VM using bhyve on FreeBSD
# Author: Vivek Gite {https://www.cyberciti.biz} under GPL v2.x+
-------------------------------------------------------------------------
# Lazy failsafe (not needed but I will leave them here)
ifconfig tap0 create
ifconfig em0bridge addm tap0
if ! kldstat | grep -w vmm.ko 
then
    kldload -v vmm
fi
if ! kldstat | grep -w nmdm.ko
then
    kldload -v nmdm
fi
bhyve -c 2 -m 2G -w -H \
 -s 0,hostbridge \
 -s 4,virtio-blk,/dev/zvol/zroot/debianvm \
 -s 5,virtio-net,tap0 \
 -s 29,fbuf,tcp=0.0.0.0:5900,w=1024,h=768 \
 -s 30,xhci,tablet \
 -s 31,lpc -l com1,stdio \
 -l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI.fd \
 debianvm

```

## vm-bhyve way 

* https://github.com/churchers/vm-bhyve
* https://www.youtube.com/watch?v=Yzi4uhDMT-s




```
pkg install grub2-bhyve bhyve-firmware uefi-edk2-bhyve vm-bhyve
```



```

[1/1] Installing vm-bhyve-1.4.2...
[1/1] Extracting vm-bhyve-1.4.2: 100%
=====
Message from vm-bhyve-1.4.2:

--
To enable vm-bhyve, please add the following lines to rc.conf,
depending on whether you are using ZFS storage or not. Please note
that the directory or dataset specified should already exist.

    vm_enable="YES"
    vm_dir="zfs:pool/dataset"

OR

    vm_enable="YES"
    vm_dir="/directory/path"

Then run 'vm init'.

sudo zfs create zroot/bhyve-vms-dataset


# vm-bhyve entries in /etc/rc.conf
vm_enable="YES"
vm_dir="zfs:zroot/bhyve-vms-dataset"




pkg install vm-bhyve
pkg install bhyve-firmware
pkg install uefi-edk2-bhyve


sudo vm init
mkdir -p  /zroot/bhyve-vms-dataset/.templates/

sudo cp /usr/local/share/examples/vm-bhyve/* /zroot/bhyve-vms-dataset/.templates/
sudo nano /zroot/bhyve-vms-dataset/.templates/debian-jacek-uefi.conf


loader="uefi"
cpu=2
memory=2048M
network0_type="virtio-net"
network0_switch="public"
disk0_type="ahci-hd"
disk0_name="disk0.img"

graphics="yes"
graphics_res="1440x900"
graphics_port="5901"
xhci_mouse="yes"




#already exist
#

# sudo vm switch create public
# sudo vm switch add public em0 
# is failing: 
#/usr/local/sbin/vm: ERROR: failed to add member em0 to the virtual switch public

sudo vm switch create -t manual -b em0bridge public

# byhve and jail settings bridges 
#cloned_interfaces="bridge0 tap0"
#ifconfig_bridge0_name="em0bridge"
#ifconfig_em0bridge="addm em0 addm tap0 up"




sudo vm switch list 


#vm create myguest
sudo vm iso https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.1.0-amd64-netinst.iso
sudo vm create -t debian-jacek-uefi -s 60G debian-11-uefi
sudo vm install -f debian-11-uefi debian-11.1.0-amd64-netinst.iso


# cd /boot/efi/EFI
# mkdir BOOT
# cp debian/grubx64.efi BOOT/bootx64.efi


#
sudo vm console debian-11-uefi
```





## by handbook 

* preparations

```
sudo pkg install grub2-bhyve bhyve-firmware uefi-edk2-bhyve


```
* add to f/etc/rc.conf' file (I am enabling few tap interfaces ans I am planning to run few virtual machines at once ): 
```
# bhyve
# byhve and jail settings bridges 
cloned_interfaces="bridge0 tap0 tap1 tap2"
ifconfig_bridge0_name="em0bridge"
ifconfig_em0bridge="addm em0 addm tap0 addm tap1 addm tap2 up"

```

* enable tap interfaces on bootup

```
su -
sysctl net.link.tap.up_on_open=1
net.link.tap.up_on_open: 0 -> 1
echo "net.link.tap.up_on_open=1" >> /etc/sysctl.conf

reboot 

# after reboot run ifconfig  to check if all inrterfaces are up
# expected new interfaces: 
# bridge0 tap0 tap1 tap2
```



### debian_1 

* this  debian_1 vm will be using tap1 interface

```
mkdir -p bhyve/debian-11
cd bhyve/debian-11

# I need a disk for it
sudo truncate -s 60G linux.img

# and I need iso installer 
wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.1.0-amd64-netinst.iso


# create device map file `device.map`: 

(hd0) ./linux.img
(cd0) ./debian-11.1.0-amd64-netinst.iso


#install
sudo grub-bhyve -m device.map -r cd0 -M 1024M  debian_1

sudo bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap1 -s 3:0,virtio-blk,./linux.img \
    -s 4:0,ahci-cd,./debian-11.1.0-amd64-netinst.iso -l com1,stdio -c 1 -m 1024M debian_1
    
sudo bhyvectl --destroy --vm=debian_1


# running 

sudo grub-bhyve -m device.map -r hd0,msdos1 -M 2048M debian_1

sudo bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap1 \
    -s 3:0,virtio-blk,./linux.img -l com1,stdio -c 4 -m 2048M debian_1
   
sudo bhyvectl --destroy --vm=debian_1


```


## old notes 


```
su - 


# ifconfig tap0 create
# sysctl net.link.tap.up_on_open=1
net.link.tap.up_on_open: 0 -> 1
echo "net.link.tap.up_on_open=1" >> /etc/sysctl.conf


# ifconfig bridge0 create
# ifconfig bridge0 addm em0 addm tap0
# ifconfig bridge0 up


sudo pkg instal 
sudo pkg install grub2-bhyve bhyve-firmware uefi-edk2-bhyve



pkg install grub2-bhyve bhyve-firmware uefi-edk2-bhyve vm-bhyve
pkg install bhyve-firmware
pkg install uefi-edk2-bhyve


truncate -s 60G linux.img

device.map
(hd0) ./linux.img
(cd0) ./debian-11.1.0-amd64-netinst.iso


grub-bhyve -m device.map -r cd0 -M 4096M linuxguest

```

