---
date: "2018-10-19T17:38:25+07:00"
title: Plex Media Server Setup Guide with Raspberry Pi
tags: [tutorial, raspi]
---

Do you wish to have your own Netflix (but better) with all of your favorite movies, shows, videos, music, photos, podcasts, and all personal media in one place, ready to stream and serve you anytime? Well, let me introduce you to **Plex**

## Objective

- Install new OS to Raspberry Pi
- Install new package
- Have your personal media server 24/7
- Stream anywhere anytime
- Manage your device/server anywhere anytime

### Materials

- Raspberry Pi 3 Model B+ (Minimum)
- MicroSD card (4/8/16/32 GB)
- Preferred size of External Hard Drive (1TB/2TB/4TB)
- USB-A to micro usb cable and a power adapter
- Powered USB hub

### Tools

- Active internet connection
- Windows PC
- Monitor with HDMI input
- HDMI cable
- Mouse and keyboard

<!-- content -->

### PREREQUISITE &bull; Installing (Fresh) Raspbian

1. [Windows] Format your microSD card, preferrably from [SDCard.org](https://www.sdcard.org/downloads/formatter_4/)
2. [Windows] Choose FAT32 for its file system as written in the [docs](https://www.raspberrypi.org/documentation/installation/sdxc_formatting.md)
3. [WIndows] Download [Raspbian with NOOBS](https://www.raspberrypi.org/downloads/noobs/)
4. [RasPi] Insert microSD card to RasPi, HDMI to monitor, everything else you need, **then** plug-in the power last
5. [RasPi] Choose Raspbian as your OS and English for your language

## Note

Some users had problems with `wlan0` once Raspbian is installed. To avoid this problem, try setting up the wifi under network near the top of the window before installing

### PART A [RasPi] &bull; Plex Media Server

1. Update and upgrade your packages

    ```bash
    sudo apt update
    sudo apt upgrade
    ```

2. Install the package HTTPS transport so you can access metadata and packages over HTTPS and use the next command line

    ```bash
    sudo apt install apt-transport-https
    ```

3. Adding dev2day repository for the `plexmediaserver` package by first adding the GPG key

    ```bash
    wget -O - https://dev2day.de/pms/dev2day-pms.gpg.key | sudo apt-key add -
    ```

4. Edit the package list using echo and tee command

    ```bash
    echo "deb https://dev2day.de/pms/ jessie main" | sudo tee /etc/apt/sources.list.d/pms.list
    ```

5. Update your packages just to make sure

    ```bash
    sudo apt update
    ```

6. Installing Plex Media Server from dev2day repository

    ```bash
    sudo apt install -t jessie plexmediaserver
    ```

7. Change `plexmediaserver.prev` so that it runs under `pi` user

    ```bash
    sudo nano /etc/default/plexmediaserver.prev
    ```

8. Save and exit with **Ctrl + X &rarr; y &rarr; Enter**
9. Look for this piece of code `PLEX_MEDIA_SERVER_USER=plex` and change `plex` to `pi`
10. Restart Plex service using the following command

    ```bash
    sudo service plexmediaserver restart
    ```

### PART B [RasPi] &bull; Static IP

1. Find your IP from this command using `hostname -I`
2. Copy and paste the IP as `ip=YOUR_IP` in `/boot/cmdline.txt`
3. Save and exit with **Ctrl + X &rarr; y &rarr; Enter**
4. Because we're running JESSIE, we need to configure `dhcpcd.conf` too using `sudo nano /etc/dhcpcd.conf`
5. Add these lines of code to the bottom of the file

    ```bash
    # change eth0 to wlan0 or add it if you use wifi
    interface eth0
    static ip_address=192.168.xxx.xxx/24
    static routers=192.168.x.x
    static domain_name_servers=8.8.8.8

    # Code below is to disable automatic configuration of empty option
    # Add this if you are experiencing trouble with your network connection endlessly configuring
    denyinterfaces eth0

    # Code below is to disable IPv6 so you'll only have your IPv4 as the static IP
    noipv6
    ```

6. Restart your RasPi to reconfigure everything `sudo reboot`

### PART C [RealVNC] &bull; Virtual Network Computing

Even if you're going to work with a monitor, it is convenient to have a VNC connection just because you would save a lot of hassle in the long run. But, it is definitely a must for users who are planning to go full headless

1. Install RealVNC server

    ```bash
    sudo apt install real-vnc-server
    ```

2. Install RealVNC viewer if you're planning to connect to other device from this device

    ```bash
    sudo apt install real-vnc-viewer
    ```

3. Setting raspi-config to allow VNC connection

    ```bash
    sudo raspi-config
    ```

4. Navigate to Interfacing Options and enable the VNC server

    ![Raspberry Pi Main Config](./raspi-config-main.jpg)

    ![Raspberry Pi Interfacing Config](./raspi-config-interfacing.jpg)

    ![Raspberry Pi VNC Config](./raspi-config-vnc.jpg)

5. Open RealVNC from the top right icon and log in with your account
6. Now, every time you want to connect to any of your device from any device, just log in and click on your device

    *Note* - Default RasPi username is `pi` with `raspberry` as the password

### TRANSPORTING &bull; Personal Media

*Tip* - Formatting your external drives beforehand and giving each of them a unique name will make it easier by not having you to go through lots of unnecessary mounting process, I recommend these so it's just 'Plug and Play'

1. Add your media folders to your external hard drive, I suggest structuring your folders like so

    ```
    E:.
    ├───Movies
    │   ├───Animated
    │   │   ├───AnimatedMovie A
    │   │   └───AnimatedMovie B
    │   └───Live Action
    │       ├───Movie A
    │       └───Movie B
    ├───TV Shows
    │   ├───Show A
    │   └───Show B
    ├───Videos
    │   ├───Video A
    │   └───Video B
    ├───Music
    │   ├───MusicAlbum A
    │   └───MusicAlbum B
    └───Photos
        ├───PhotoAlbum A
        └───PhotoAlbum B
    ```

    *Tip* - Structuring your media will help Plex scan and detect your media files better and save the hassle of manually correcting it later

2. Once done, eject your drive and plug it in to your RasPi

    *Important* - Because we're using external drives to store our media, we'll want to use a powered USB hub to prevent your RasPi from being underpowered and slowing down its performance (which is slow enough compared to actual NAS) and/or even risk getting shut down and bricked

    *Note* - If you don't have a powered USB hub yet and are only using 1 external drive, you might get away by plugging it directly in to the RasPi. But, try to get one as soon as possible

3. You can check your Plex on your RasPi at `192.168.xxx.xxx:32400` or `localhost:32400` will do just fine too
4. Click the `Add Library` under Libraries, configure General and Advanced to your likings, and browse for a folder on your external drive at `/media/pi/external_drive`

#### Troubleshooting

1. Your external drive might not be compatible to read with NTFS as its file system and a storage of over 2GB as we've previously added a lot of our personal media from Windows to the drive.

    To resolve this problem, we could install NTFS-3G. It is an open source cross-platform  implementation of the Microsoft Windows NTFS file system with read-write support

    ```bash
    sudo apt install ntfs-3g
    ```

    If you're using an external storage but with exFAT as its file system and are having problems. You could install `exfat-fuse` and `exfat-utils`

    ```bash
    sudo apt install exfat-fuse exfat-utils
    ```

    *Important* - Do not use exFAT for your external hard drive if you are using that to keep your `Plex Metadata`, which needs symlinks support and that is not provided by exFAT

2. You might not be able to access your drive(s).

    To resolve this problem, we could do a chmod 777 to make the files readable, writable, and executable by everyone with `sudo chmod 777 /media/pi`

### SERVER MANAGEMENT &bull; Dataplicity

An alternative to manage your mini RasPi home server is to use [**Dataplicity**](https://www.dataplicity.com/). Usually, you don't need to have access to the mouse movement or even the display for a Linux-based OS, you only need to have access to the Terminal. It is that powerful that you could basically do anything from it, and that is where **Dataplicity** comes. It gives you full access to just the Terminal without adding any additional load from having to display any visuals or waiting for the mouse to slowly go where we wanted to. It is the (perfect for me) lightweight and hassle-free replacement for SSH (which I don't use and prefer this one).

1. Go to [**Dataplicity**](https://www.dataplicity.com/)'s main page and enter your email
2. You will then be given a curl code to be executed from your RasPi first, example code below

    ```bash
    curl https://www.dataplicity.com/abcdefg0.py | sudo python
    ```

3. You should be done with your RasPi. To access your device, go back to **Dataplicity** and select your device
4. You could see that a huge Terminal has showed up for you to use and everything you enter there will be executed in your device
5. You should notice that it starts with something like `dataplicity@your-device:/$`, it means you're on `Dataplicity` user and are most likely not a superuser. You could change user with `su pi`
6. Enter your `pi` password
7. Use **Ctrl + Shift + V** to paste something from Windows
8. You could now manage your server anywhere and anytime just by visiting their website, logging in, and use the cheat sheet I've provided down below to execute the essentials
    *Tip* - If you're having trouble connecting, try enabling **SSH** from raspi-config the same way you enabled VNC before

### CHEATSHEET &bull; Server Management Commands

```bash
sudo apt update
sudo apt upgrade

sudo service plexmediaserver stop
sudo service plexmediaserver start
sudo service plexmediaserver restart

sudo reboot # Reboot Raspi
sudo tuptime # Shows RasPi uptime
sudo shutdown -h now # Shutdown now

sudo apt install plexmediaserver-install # Update Plex

iwconfig # Wifi Strength
```
