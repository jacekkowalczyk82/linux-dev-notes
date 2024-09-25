
bundle27 exec jekyll post "How to create USB stick with Kali Linux and persistence" 




I have got 16GB SanDisk Cruzer Fit USB stick, and I want to install Live Kali linux on it with persistence. 

I used Balena Etcher to install live kali image to USB stick. 

Next I started configuring the persistence partition following the official tutorial from [Kali Linux  blog](https://www.kali.org/docs/usb/usb-persistence/)

```
sudo fdisk --list 

# which gave me the output 

Disk /dev/sdb: 14,33 GiB, 15376318464 bytes, 30031872 sectors
Disk model: Cruzer Fit      
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xf6bb8fe4

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdb1  *         64 7012351 7012288  3,4G 17 Hidden HPFS/NTFS
/dev/sdb2       7012352 7013823    1472  736K  1 FAT12

```

I switched to root account 

```
sudo su - 
```

and changed the work directory 
```
cd /home/jacek/Downloads/
```

Newxt I executed the commands sugested by the tutorial from the kali linux blog. 

```
read start _ < <(du -bcm kali-linux-2021.1-live-amd64.iso  | tail -1); echo $start
3426
end=7GiB
parted /dev/sdb mkpart primary ${start}MiB $end
fdisk --list 
```

Using these commands I created 3rd partition with size 3.7 GB.  I was trying to adjust start and end parameters to get 4GB partition, but it was quite difficult. 
I decided to use cfdisk tool to create 3rd partition with exact size 4GB. If I am not worong this is the maximal size allowed for persistence partition. But later on I thought is it really 4GB a maximal size? Lets try to use all remaining space and create bigger partition. So I finally created partition with size 11GB. 

In the last step I needed to infom my live kali instance that the USB stick has a persistence partition: 

```
mkfs.ext3 -L persistence /dev/sdb3
e2label /dev/sdb3 persistence
mkdir -p /mnt/my_usb
mount /dev/sdb3 /mnt/my_usb
echo "/ union" > /mnt/my_usb/persistence.conf
umount /dev/sdb3
```

To test it I inserted the USB stick to laptop and booted it. I selected Live image with persistence on the GRUB screen. 




   24  start=3400
   25  end=7400
   26  parted /dev/sdb mkpart primary ${start}MiB $end
   27  end=7.4GiB
   28  parted /dev/sdb mkpart primary ${start}MiB $end
   29  fdisk /dev/sdb
   30  end=7.4GiB
   31  parted /dev/sdb mkpart primary ${start}MiB $end
   32  fdisk /dev/sdb
   33  end=7.7GiB
   34  parted /dev/sdb mkpart primary ${start}MiB $end
   35  fdisk /dev/sdb
   36  mkfs.ext3 -L persistence /dev/sdb3
   37  e2label /dev/sdb3 persistence
   38  mkdir -p /mnt/my_usb
   39  mount /dev/sdb3 /mnt/my_usb
   40  echo "/ union" > /mnt/my_usb/persistence.conf
   41  umount /dev/sdb3
   
   
   
   
   
 1958  read start _ < <(du -bcm kali-linux-2020.4-live-amd64.iso | tail -1); echo $start
 1959  cd Downloads/
 1960  read start _ < <(du -bcm kali-linux-2020.4-live-amd64.iso | tail -1); echo $start
 1961  sudo fdisk /dev/sdb
 1962  end=7.3GiB
 1963  read start _ < <(du -bcm kali-linux-2020.4-live-amd64.iso | tail -1); echo $start
 1964  sudo parted /dev/sdb mkpart primary ${start}MiB $end
 1965  sudo bash 
 1966  sudo fdisk --list 


