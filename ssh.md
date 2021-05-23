# ssh Basics

Needed to connect to my more poweful desktop from my macbook - this is how:

## Devices on same network

### Enable SSH on Ubuntu

Followed [this tutorial](https://devconnected.com/how-to-install-and-enable-ssh-server-on-ubuntu-20-04/)

SSH should be installed by default. You can check by running `ssh -V`. You then need to install openssh-server

```
sudo apt-get update
sudo apt-get install openssh-server
```

Then check the status to make sure it works:

`sudo systemctl status sshd`

It should say "active". You then need to enable ssh traffic on the firewall:

`sudo ufw allow ssh`

### Connecting to computer via ssh

On mac, ssh is enabled by default. So I simply ran:

`ssh USERNAME@ip_address`

where USERNAME is the username on the *target* machine. IP address is also of the target machine, and can be found by typing `ip a`

### Running a Jupyter Notebook

You need to **link the ports on the host and target machines**. You do this **when connecting via ssh**:

`ssh username@xx.xx.xx.xx -NL 1234:localhost:1234`

The `-NL 1234:localhost:1234` is the key here


## General

* To exit the ssh tunnel: type `exit`
