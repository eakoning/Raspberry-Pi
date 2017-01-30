# Raspberry Pi

Installation of Raspbian Jessie Lite

https://www.raspberrypi.org/downloads/raspbian/

## Raspbian configuratie
Basisconfiguratie van Raspbian aanpassen.

`sudo raspi-config`

In principe hoef je niets te configureren in je Raspberry om een USB WiFi adapter te installeren. Om te kijken of de adapter WLAN's herkent, kun je het volgende statement gebruiken om te scannen en alle beschikbare SSID's te tonen.

`sudo iwlist wlan0 scan | grep ESSID`

## Keyboard configuration
- Internationalisation Options
- Change Keyboard Layout
- Dell 101-key PC
- English (US) - English (US, with euro on 5)
- Right Alt (AltGr)
- No Compose key

## clean up input history
history -c

## aliasen
in de home folder (~) vind je het .bashrc bestand.

daarin zijn de aliasen gedefinieerd.  
wijzigingen kun je doen dmv `sudo nano ~/.bashrc`

> Opnieuw laden van settings in je .bashrc file kan dmv de **dot source notatie**.  
_Let op de punt (vandaar de dot) waarmee het commando begint._

`. ~/.bashrc`

## config.txt aanpassen
Makkelijkste via `sudo nano /boot/config.txt

## Herstart
`sudo reboot`

## Afsluiten
`sudo shutdown -h now`

## Netwerk configuratie opvragen
`ifconfig`

#Adafruit examples

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