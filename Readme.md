# Raspberry Pi

Some information for the raspberry pi:

![](https://raw.githubusercontent.com/Sylhare/RaspPI/master/resources/raspberry-pi-small.gif)

## Getting Started

### Install the OS

To [start](https://projects.raspberrypi.org/en/projects/raspberry-pi-getting-started) with a Raspberry Pi, you first need to install an image of an OS in a micro SD card.

- Get the OS on the [raspberry website](http://www.raspberrypi.org/downloads/)
    - Raspbian is a good choice.
- Get the OS on the micro SD using a tool like [live linux](https://www.linuxliveusb.com/fr/download) or [Win32DiskImager](http://sourceforge.net/projects/win32diskimager/)
    - If you have any problem of memory with the SD card, use [SDformatter](https://www.sdcard.org/downloads/formatter_4/)
    
### Set a default IP

In order to easily connect to the raspberry pi over ssl or ethernet, you may want to set it with a default ip.

For that edit the file called `cmdline.txt` at the root of your SD card and set the ip parameter like `ip=169.254.25.25`.

You can choose anything between 169.254.0.0 to 169.254.255.255 (it's a range of [local private ip](https://www.lifewire.com/automatic-private-internet-protocol-addressing-816437)).

### Connect to the Raspberry Pi

The default login password is:
```
login: pi
password: raspberry
```
> Be wary that if your raspberry pi is open on the internet with a default password, somebody could get access to your home network and mess with it.

You can connect the pi through the HDMI port, or via ssl using an ethernet cable between the rasbperry pi and your pc or directly over wifi in your home network.

The tools you may use:
    
- The terminal via ssh e.g.:
```
ssh pi@169.254.25.25 -p 22
```
- Using [Putty](https://www.putty.org/):
   - Use Session > logging, then put the ip address it will ask the username, password.
- Using a remote desktop software line [VNC](https://www.realvnc.com/fr/):
    - you will need to install it both on your computer and on the Raspberry pi
    
> If you have just turned on the raspberry pi, wait a couple of seconds for it to boot up before trying to connect over ssl.

You can also configure the static ip in the `ect/network/interfaces` config file.

### Set up the wi-fi dongle

For the previous generation of Raspberry pi there was no dongle integrated, and it needed to be bought independently.

To configure it, find some example configuration files in this repo. Once you made a change use `ifconfig` to check your `wlan0` has been updated.

#### Default Method

Find the default configuration for the interface, then:

```
sudo nano /etc/network/interfaces
```
Then add this to the file, don't forget to update your wifi SSID name and password to connect.

```config
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-ssid "<SSID name>"
wpa-psk "<password>"
```

Apply the changes with:
```
sudo ifdown wlan0
sudo ifup wlan0
```

#### WPA supplicant Method

This one basically uses another configuration file (both in the wifi folder of this repository).

Update the interface file like before, using the correct interface information.
Then create the wpa_supplicant.conf file with:
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Don't forget to reboot to validate the changes:
```
sudo reboot
```
or
```
shutdown -r now
```

### Connecting using VNC

You get to have the gui and the control of the raspberry pi.

## General configuration

### Configure the raspberry pi

This is to access the main configuration for the raspberry pi:
```
sudo raspi-config
```
You can then follow the instructions on the screen to make the update.
Start the graphical interface if available using:
```
sudo startx
```

### Refresh the OS

To make sure you have all of the latest update on the OS you have just installed use:
```
sudo apt-get update
sudo apt-get upgrade
```
You can also make a dependency check using 
```
sudo apt-get dist-upgrade
```
Or you can also add an auto accept flag to validate all of the required insallation
```
sudo apt-get update -y && sudo apt-get upgrade -y
```

### Set the date

Sometime the date on the raspberry pi can be out of sync if not connected to a time server in your network or on the internet.
To change the time, you can either use for example:
```
sudo date --set="30 December 2013 10:00:00"
```
or
```
timedatectl set-time [THE DATE]
```

To change the time zone, you can use:
```
sudo dpkg-reconfigure tzdata 
```

### Configure the Raspberri Pi

Go into the terminal and then

```
sudo raspi-config
```
You can follow the instruction for base cofniguration.



## Start Coding

### Processing

The raspberry pi has a built in Processing IDE.

- Get Started with [Processing](https://projects.raspberrypi.org/en/projects/introduction-to-processing/3)

## Raspberry Pi projects

### Retro Gaming

To enable your raspberry PI to become a retrogaming console using [Retropie](https://retropie.org.uk/)

### Media Center

Transform your raspberry pi into a media center, streaming movies from a hard drive or Youtube, ...

It was called XBMC first, but has been rename to [kodi](https://kodi.wiki/view/HOW-TO:Install_Kodi_on_Raspberry_Pi).

- [OSMC](https://osmc.tv/) is a good one.


## Install a Apache, PHP, MySQL web server

### Configure Apache

First install apache: (Resolving error AH00558)

```
sudo apt-get install apache2
```
Edit the config of the apache:

```
sudo nano /etc/apache2/apache2.conf
```
to set the server to localhost, print that at the end of the file, localhost is `127.0.0.1`

```
ServerName localhost
```

Then give read qccess to apache on the folders where the files that apache will run are (usually in a `/www` folder)

```bash
sudo chown -R www-data:pi /var/www/html/
sudo chmod -R 770 /var/www/html/
```

Verify that it works (ssh):

```bash
wget -O verif_apache.html http://127.0.0.1
cat ./verif_apache.html  # if the file exists, then apache has been successfully installed
```

### Configure PHP

First install php:

```
sudo apt-get install php v5
```
Delete any `index.html` in the `/www` folder, so you can test that php works

```bash
sudo rm /var/www/html/index.html  # delete index.html
echo "<?php phpinfo(); ?>" > /var/www/html/index.php  # create index.php
```

Verify that php works:

```bash
wget -O verif_apache.html http://127.0.0.1
cat ./verif_apache.html  # if you see php version 5.x.x then it works
```

### Instal MySQL and tools

Here are some tool you will need to install in order to better use your webserver.
It is mainly to interact between your php and the MySQL database.

```
sudo apt-get install mysql-server --fix-missing
sudo apt-get install mysql-client php5-mysql
sudo apt-get install phpmyadmin
```
