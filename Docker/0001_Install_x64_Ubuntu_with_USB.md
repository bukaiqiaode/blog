# Steps needed to install x64 Ubuntu using USB stick

**64-bit** versions are needed to using Docker, according to the [Prerequisites](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/) of installing Docker on Ubuntu.

## Step 1
Download the ISO image of Ubuntu from [Ubuntu site](https://www.ubuntu.com/download/desktop) <br/>
Here, I used **ubuntu-16.04.3-desktop-amd64.iso**

## Step 2
It is easy to do installation with one USB stick. <br/>
Plug the USB to the PC(running Ubuntu), and check the device number assigned. Here it is `/dev/sdb`.<br/>
From the command-console, copy the ISO file directly to the USB, not to any partition.
```shell
  sudo cp ubuntu-16.04.3-desktop-amd64.iso /dev/sdb
  sudo sync
```

## Step 3
Reboot the PC with USB plugged, then proceed to install Ubuntu.

## References
- https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
- https://help.ubuntu.com/lts/installation-guide/amd64/install.en.pdf

## Back to [index](./index.md)
