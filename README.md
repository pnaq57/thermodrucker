thermodrucker
=============
#change rechte
sudo chmod 777 /etc/usb/lp0

#Wechsel in das Verzeichnis
cd /etc/ifplugd/action.d/

#Datei ifupdown in ifupdown.original umbenennen
mv ifupdown ifupdown.original

#Kopiere dann die Datei
#/etc/wpa_supplicant/ifupdown.sh
#als ifupdown in das aktuelle Verzeichnis –
cp /etc/wpa_supplicant/ifupdown.sh ./ifupdown

#Dadurch wird die bisherige ifupdown durch eine neue Version ersetzt, welche das WLAN-Netzwerk nach einem Verbindungsverlust automatisch wieder reaktivieren kann.
sudo reboot


#!/bin/bash

ROUTER=a.b.c.d
LOG=/var/log/netzwerk.log

if ping -c 1 -W 3 -I eth0 -q $ROUTER; 
then 
        echo `/bin/date` Netzwerk in Ordnung >> $LOG
        echo `/sbin/ip neigh show` arp cache table >> $LOG
else
        echo `/sbin/ip neigh show` arp cache table >> $LOG
        echo `/bin/date` Starte Netzwerk neu >> $LOG
        /etc/init.d/networking restart
        /sbin/mii-tool -F 10baseT-HD
fi

# mit option  n und e die ganze Zeile Leerzeichen zu beüllen, damit es gedruckt wird
sudo echo -n -e "test zeile\r">/dev/usb/lp0
