# Raspberry Pi

## Introduction

This document describes various steps that were taken by me to get a working installation of Raspbian Jessie Lite with Docker running different containers.

https://www.raspberrypi.org/downloads/raspbian/

## Raspbian configuration
Raspbian has a nice User Interface for changing some OS defaults.
Start it with the command

`sudo raspi-config`

The changes you might want to make

* Expand Filesystem
* Change User Password
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

## Tips and tricks

### Clear input history
`history -c`

### Aliasses
Predefined aliasses are defined in the `~/.bashrc` file in your home folder, hence the `~`.
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

### Netwerk configuratie opvragen
`ifconfig`

# Adafruit examples

## Prerequisites installeren

* Pyton-dev
* GPIO

`sudo apt-get install python-dev python-rpi.gpio`

> Voor installatie van de LCD Module open je de map  
 **/home/pi/Adafruit_Python_CharLCD**  
 Vanuit die map start je het volgende commando:

`sudo python setup.py install` 

## Terug naar de prompt

Veel commando's (waaronder python) starten een nieuwe command line interface. Om weer terug te keren naar je bash command prompt kun je **Ctrl-D** gebruiken.

#Adafruit onderdelen

## 18B20 Thermometer sensoren gebruiken
Je zult de Device Tree van de Pi moeten configureren voor gebruik met de 18B20 modules.  
Voeg `dtoverlay=w1-gpio toe aan config.txt  
Default wordt de w1 module geïnitialiseerd op GPIO pin 4. Zorg dus dat ze daarop worden aangesloten. 

## Libraries
Een gemakkelijke library voor het uitlezen van deze sensoren is de **w1thermsensor**
https://github.com/timofurrer/w1thermsensor  
Voor het installeren gebruik je:  
`sudo apt-get install python-w1thermsensor`

# Troubleshooting

## PuTTY Fatal Error
Network error: Connection refused
Make sure you can ping your raspberry.
Try logging in on the raspberry directly by connecting a monitor and a keyboard and use 
`ifconfig`

When connected through Ethernet you should see `eth0` in the list showing an IP address after `inet addr:`.
This is the address you should be able to use for connecting with SSH, if your machine is on the same subnet.