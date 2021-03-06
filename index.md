# Raspberry Pi

## Setup

### Download Raspbian
https://www.raspberrypi.org/downloads/raspbian/

Install on SD Card with for instance *Win32DiskImager*.

### Enabling SSH
SSH is disabled by default on Raspbian. To be able to connect to the Raspberry through SSH, you can either

1. Enable it through raspi-config or
2. Place a file called `ssh` in the `/boot/` folder of the FAT32 partition on the SSD card. After booting up SSH should be enabled.

## Raspbian configuration
Raspbian has a nice User Interface for changing some OS defaults.
Start it with the command

`sudo raspi-config`

The changes you might want to make

* Expand Filesystem
* Change User Password (alternatively do this through the command line with the `passwd` command)
* Internationalisation Options
    * Change Locale
    * Change Timezone
    * Change Keyboard Layout
* Advanced Options
    * SSH - Enable

> For reference, the keyboard configuration I use is 
Dell 101-key PC, English (US) - English (US, with euro on 5)

## Configuring Wi-Fi USB adapter
When a supported USB Wi-Fi adapter is used, nothing needs to be installed in Raspbian to get it working.
Use the following command to have it search for available networks.
By using the grep pipe, you can filter down on a specific SSID.

`sudo iwlist wlan0 scan | grep ESSID`

The file `/etc/wpa_supplicant/wpa_supplicant.conf` holds the wireless network configurations.

It's possible to create such a file with the following command:
(this command only works as root, use `sudo su` to change user, when running as root, changing back is as simple as `su pi` or whatever user you're using).

`wpa_passphrase MyNetwork MyPassword >> /etc/wpa_supplicant/wpa_supplicant.conf`

> Be advised that the generated file does actually contain the password, but it's commented out. Modify with `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`.

## Updating existing image with latest bits and pieces
```
sudo apt update
sudo apt full-upgrade
```

## Tips and tricks

### Command Line
`history -c` clears the complete history.

`history [n]` shows the history where [n] (optional) specifies the last number of items to show.

`![number]` executes a specific item from the history.

`!!` repeat the last command.

`history | grep [Text]` finds all commands with the specified (or part of) text.

`![text]` will repeat the **last** command starting with the specified text.

`Ctrl-R` starts reverse search in history. Type a few characters to search and press `Ctrl-R` to loop through the results. Press `Enter` to execute the selected command. Use arrow `left` or `right` to display the command on the command line to edit it.

`Ctrl-U` clears the command line.

### Aliases
Predefined aliases are defined in the `~/.bashrc` file in your home folder, hence the `~`.
Easily edit this file with the command

`sudo nano ~/.bashrc`

> Reloading the settings of the ~/.bashrc file can be done by using a dot.

`. ~/.bashrc`

### Edit config.txt from inside SSH
This can be easily done through `sudo nano /boot/config.txt`

### Rebooting the device
`sudo reboot`

### Shutting down the device
`sudo shutdown now`

### Check Network interface information (IP Address etc)
`ifconfig`

`ifconfig wlan0` (for a specific interface)

### Check Which Wireless network is being used
`iwconfig`

### Restart network
`sudo wpa_cli reconfigure`

# Adafruit examples

## Installing Prerequisites

* Pyton-dev
* GPIO

`sudo apt-get -y install git python-dev python-rpi.gpio`

> For installation of the LCD Module, open the folder
 `/home/pi/Adafruit_Python_CharLCD`
 Start the following command

`sudo python setup.py install` 

## Back to Bash prompt

A lot of commands, for instance python, will start a new command line interface. To return to the bash prompt, use `Ctrl-D`.

## Using 18B20 temperature sensors
The *Device Tree* of the Pi needs to be configured for usage with the 18B20 modules.  

Add `dtoverlay=w1-gpio` to the `/boot/config.txt` file residing on the FAT32 partition of the SD Card.

Editing can be done with the following command: `sudo nano /boot/config.txt`. **Don't forget to reboot after editing the config.txt**.

> By default the w1 module will be initialised on **GPIO pin 4**. Make sure the sensors will be connected to this pin.

### Testing the sensors

```
sudo modprobe w1-gpio
sudo modprobe w1-therm
cd /sys/bus/w1/devices
ls
cd 28-xxxx (change this to match what serial number pops up)
cat w1_slave
```

The contents of the file will show the temperature measured by the sensor.

## 3rd Party Libraries
A convenient library for reading these sensors is [**w1thermsensor**](https://github.com/timofurrer/w1thermsensor).

To install, run

`sudo apt-get install python-w1thermsensor`

# Troubleshooting

## PuTTY Fatal Error
Network error: Connection refused

**Solution:** Please bear in mind that SSH is disabled by default on Raspbian. [Enabling SSH](#enabling-ssh)

Still unable to connect? You might need to connect a keyboard and a monitor to your Pi.

# Installing Node

Untested with Raspberry Pi Zero W (which turns out to run on ARM6 and does not work with the install script as described on nodejs.org).

```
sudo apt-key adv --keyserver keyserver.[debian ??].com --recv 68576280 <-- which number should this be??
sudo apt-add-repository "deb https://deb.nodesource.com/node_7.x $(lsb_release -sc) main"
sudo apt-get update
sudo apt-get install nodejs
```
[How to Install the Latest Versions of NodeJS and NPM for Ubuntu 14.04 LTS](http://askubuntu.com/questions/594656/how-to-install-the-latest-versions-of-nodejs-and-npm-for-ubuntu-14-04-lts)

# Debian Jessie Pixel

## Tips & Tricks

No mouse available? There are certain shortcuts available.

- Open a Terminal (_LXTerminal_) with the shortcut `Ctrl-Alt-T`.