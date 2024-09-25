
# Canonical Ubuntu https://multipass.run/

What is multipass, it is a toola that allows to easily create and use Ubuntu server Virtual  Machines at Windows, Linux or MacOS. 

Install multipass for your platform following instructions at `https://multipass.run`

## Create and start new VM machine 

```
multipass delete ubuntu-1 --purge

multipass launch --name ubuntu-1 --cpus 2 --disk 20G --mem 2G 
multipass shell ubuntu-1
```


## Inside ubuntu-1 VM install applications and create mounting point directory 

```
mkdir -p ~/ubuntu-1-home

sudo apt update  && sudo apt install xorg mc geany firefox tmux git 


```

## On host system create mounting point that will be shared with guest VM 

**On primary VM - This is not needed as the host home directory is automatically monthed into VM at ~/Home**

```
mkdir -p private/multipass/ubuntu-1-home
multipass mount private/multipass/ubuntu-1-home ubuntu-1:ubuntu-1-home
```

## SSH Connection with X11 forwarding 

**PREREQUISITE: Install your public ssh key to the VM `~/.ssh/authorized_keys`** 

```
# on host run to get the list of installed VMs
multipass list 

# allow X11 forwarding to your host 
xhost + 

# and then connect with X11 forwarding to selected VM by its IP address:

ssh -X ubuntu@192.168.64.2

# and no you can run gui apps and forward them to your display , for example: 

firefox 

```


## docker desktop replacement :-) 

```
multipass launch --name docker-runner --cpus 4 --disk 50G --mem 4G 
multipass list 


```


