# Raspberry Pi 3 image spec

**NOTE that this image is currently looking for a new maintainer:
https://people.debian.org/~stapelberg/2018/06/03/raspi3-looking-for-maintainer.html**

This repository contains the files with which the image referenced at
https://wiki.debian.org/RaspberryPi3 has been built.

## Option 1: Downloading an image

See https://wiki.debian.org/RaspberryPi3#Preview_image for where to obtain the latest pre-built image.

## Option 2: Building your own image

If you prefer, you can build a Debian buster Raspberry Pi 3 image yourself. For
this, first install the requirements:

 * vmdb2
 * qemu-user-static
 * qemu-system-arm64

Then, clone the repository:

```shell
git clone https://github.com/Debian/raspi3-image-spec
cd raspi3-image-spec
```

You can generate the image using this command:

```shell
umask 022
sudo env -i LC_CTYPE=C.UTF-8 PATH="/usr/sbin:/sbin:$PATH" \
    vmdb2 --output raspi3.img raspi3.yaml --log raspi3.log
```

## Installing the image onto the Raspberry Pi 3

Plug an SD card which you would like to entirely overwrite into your SD card reader.

Assuming your SD card reader provides the device `/dev/sdb`, copy the image onto the SD card:

```shell
sudo dd if=raspi3.img of=/dev/sdb bs=64k oflag=dsync status=progress
```

Then, plug the SD card into the Raspberry Pi 3 and power it up.

The image uses the hostname `rpi3`, so assuming your local network correctly resolves hostnames communicated via DHCP, you can log into your Raspberry Pi 3 once it booted:

```shell
ssh root@rpi3
# Enter password “raspberry”
```

Note that the default firewall rules only allow SSH access from the local
network. If you wish to enable SSH access globally, first change your root
password using `passwd`. Next, issue the following commands as root to remove
the corresponding firewall rules:

```shell
iptables -F INPUT
ip6tables -F INPUT
```

This will allow SSH connections globally until the next reboot. To make this
persistent, remove the lines containing "REJECT" in `/etc/iptables/rules.v4` and
`/etc/iptables/rules.v6`.

