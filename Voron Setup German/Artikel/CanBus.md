## CanBus Installation 


#### Support
Wenn ihr mir einen Kaffee ausgeben wollt: 
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/dfoure?country.x=DE&locale.x=de_DE)


### Verwendete Hardware 

 * BTT Octopus V1.1
 * BTT EBB36
 * Fly Mellow UTOC-3

###### Es sind aber auch andere Hardware-Konstellationen (Fysetc Spider, Fly UTOC-1 / UTOC-3 möglich)
 

## Elektrischer Anschluss
Hier gibt es diverse Möglichkeiten. 
Bewährt haben sich bei mir folgende:


###### :warning: Aufgrund der Übersicht, wurde die 24V Verdrahtung vom Octopus nicht dargestellt. Diese ist nach der Voron Anleitung durchzuführen

#### without 24V passthrough
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_01.png" alt="pinout" width=800 height=550>


#### 24V passthrough
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_02.png" alt="pinout" width=800 height=550>


:warning: Der Reihe nach folgende Schritte ausführen
* 1 CanBoot auf dem Rpi installieren
* 2 CanBoot Firmware erstellen und compilieren
* 3 CanBoot flashen
* 4 Klipper Firmware erstellen
* 5 can0 Schnittstelle auf dem Rpi konfigurieren
* 6 UUID vom EBB Board auslesen
* 7 EBB Klipper Firmware flashen
* 8 printer.cfg anpassen


#### 1. CanBoot auf dem Rpi installieren

```
sudo apt-get install git -y
git clone https://github.com/Arksine/CanBoot
```

#### 2.CanBoot Firmware erstellen und compilieren
```
cd CanBoot
make menuconfig
```
###### :warning: Hier bitte aufpassen, welches EBB36/42 Board verwendet wird und folgende Einstellungen verweden

#### BTT EBB36 v1.0 (Prozessor F072)

 - 8MHz crystal
 - CAN bus on PB8/PB9 
 - 8KiB offset
 - 500000 CAN bus speed
 
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/F072_CanBoot.png" alt="F076">


#### BTT EBB36 v1.1 und v1.2 (Prozessor GB01)

 - 8MHz crystal
 - Can bus on PB0/PB1 
 - 8KiB offset
 - 500000 CAN bus speed

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/GB01_CanBoot.png" alt="GB01">




#### 3.CanBoot Firmware erstellen und compilieren






#### 4.Klipper Firmware erstellen
```
cd ~/klipper
make clean
make menuconfig
```

:warning: Auch hier wieder auf die Board Versionen achten

für das BTT EBB36 v1.0 (Prozessor F072)
 - 8MHz crystal
 - CAN bus on PB8/PB9 
 - 8KiB offset
 - 500000 CAN bus speed


für das EBB36 v1.1 und v1.2 (Prozessor GB01)
 - 8MHz crystal
 - Can bus on PB0/PB1 
 - 8KiB offset
 - 500000 CAN bus speed


#### 5.can0 Schnittstelle auf dem Rpi konfigurieren
```
cd
sudo nano /etc/network/interfaces.d/can0
```

folgendes einfügen
```
allow-hotplug can0
iface can0 can static
 bitrate 500000
 up ifconfig $IFACE txqueuelen 256
 pre-up ip link set can0 type can bitrate 500000
 pre-up ip link set can0 txqueuelen 256
```
mit STRG+X beenden und mit Y bestätigen


#### 6.UUID vom EBB Board auslesen
```
cd ~/CanBoot/scripts
pip3 install pyserial
ifconfig
python3 flash_can.py -i can0 -q
```


#### 7.EBB Klipper Firmware flashen
```
python3 flash_can.py -f ~/klipper/ebb_klipper.bin -u <ebb_uuid>
```
du zuvor ausgelesene UUID einsprechend in den Befehl einsetzen

```
python3 flash_can.py -f ~/klipper/out/klipper.bin -u 330a31adf6de
```




https://github.com/bigtreetech/U2C/blob/master/Image/pinout.png

https://github.com/bigtreetech/U2C/tree/master/firmware


https://maz0r.github.io/klipper_canbus/controller/firmware_files/utoc_firmware.bin


#### 8. printer.cfg anpassen
```
[mcu EBB]
canbus_uuid: 330a31adf6de
```

```
[adxl345]
cs_pin: EBB:PB12
spi_software_sclk_pin: EBB:PB10
spi_software_mosi_pin: EBB:PB11
spi_software_miso_pin: EBB:PB2
axes_map: x,y,z

[resonance_tester]
accel_chip: adxl345
probe_points:
    150,150,20 
```

```
[extruder]
step_pin: EBB:PD0
dir_pin: !EBB:PD1
enable_pin: !EBB:PD2
heater_pin: EBB:PA2
sensor_pin: EBB:PA3
```
```
[tmc2209 extruder]
uart_pin: EBB:PA15
```
```
[probe]
pin: EBB:PB8 
```
```
[fan]
pin: EBB:PA0
```
```
[heater_fan hotend_fan]
pin: EBB:PA1 
```
```
[temperature_sensor ebb_temp]
sensor_type: temperature_mcu
sensor_mcu: EBB
min_temp: 0
max_temp: 120
```

