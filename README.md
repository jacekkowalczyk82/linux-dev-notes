# linux-dev-notes





## install sdkman.io 


```
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

## install tools 

```
sdk install ant 
sdk install maven
sdk install gradle

```

## install graaalvm 

* https://www.graalvm.org/22.1/docs/getting-started/linux/

## Install Docker Desktop 

* https://docs.docker.com/desktop/install/ubuntu/
* https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository

```
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    qemu-kvm 
    
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    

sudo apt-get update

sudo apt-get install docker-ce-cli docker-buildx-plugin docker-compose-plugin

???
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

 


cd Downloads
sudo apt-get install ./docker-desktop-4.17.0-amd64.deb 



```

* Start  Docker Destop 

* https://docs.docker.com/desktop/linux/#credentials-management
https://docs.docker.com/desktop/get-started/#credentials-management-for-linux-users


```
(base) jacek@pop-os:~/Downloads$ gpg --generate-key
gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: directory '/home/jacek/.gnupg' created
gpg: keybox '/home/jacek/.gnupg/pubring.kbx' created
Note: Use "gpg --full-generate-key" for a full featured key generation dialog.

GnuPG needs to construct a user ID to identify your key.

Real name: Jacek
Email address: jacekkowalczyk82@gmail.com
You selected this USER-ID:
    "Jacek <jacekkowalczyk82@gmail.com>"

Change (N)ame, (E)mail, or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /home/jacek/.gnupg/trustdb.gpg: trustdb created
gpg: key 0DE45C86913F8FBD marked as ultimately trusted
gpg: directory '/home/jacek/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/jacek/.gnupg/openpgp-revocs.d/FCB0AC00994E8689390F1FB90DE45C86913F8FBD.rev'
public and secret key created and signed.

pub   rsa3072 2023-03-20 [SC] [expires: 2025-03-19]
      FCB0AC00994E8689390F1FB90DE45C86913F8FBD
uid                      Jacek <jacekkowalczyk82@gmail.com>
sub   rsa3072 2023-03-20 [E] [expires: 2025-03-19]

(base) jacek@pop-os:~/Downloads$ 




(base) jacek@pop-os:~/Downloads$ pass init FCB0AC00994E8689390F1FB90DE45C86913F8FBD
mkdir: created directory '/home/jacek/.password-store/'
Password store initialized for FCB0AC00994E8689390F1FB90DE45C86913F8FBD



```

## After starting Docker Desktop 

```
docker run hello-world


docker run -d -p 80:80 docker/getting-started
```

## Removing docker desktop as my Laptop has too less RAM for it 

```
sudo apt remove docker-desktop

rm -r $HOME/.docker/desktop
sudo rm /usr/local/bin/com.docker.cli
sudo apt purge docker-desktop
```

