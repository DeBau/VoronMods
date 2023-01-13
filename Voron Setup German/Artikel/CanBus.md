# CanBus Installation 


#### Support
Wenn ihr mir einen Kaffee ausgeben wollt: 
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/dfoure?country.x=DE&locale.x=de_DE)


### Verwendete Hardware 

 * BTT Octopus V1.1
 * BTT EBB36
 * Fly Mellow UTOC-3

###### Es sind aber auch andere Hardware-Konstellationen (Fysetc Spider, Fly UTOC-1 / UTOC-3 möglich)
 
 
## Vorbereitungen
### Flasht euer Board ganz normal wie in der Voron Anleitung angegeben und bindet es als MCU mit der Serial in die printer.cfg ein
- [Voron Design ](https://docs.vorondesign.com/build/software/#firmware-flashing)



## Elektrischer Anschluss
Hier gibt es diverse Möglichkeiten. 
Bewährt haben sich bei mir folgende:


###### :warning: Aufgrund der Übersicht, wurde die 24V Verdrahtung vom Octopus nicht dargestellt. Diese ist nach der Voron Anleitung durchzuführen

#### without 24V passthrough
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_01.png" alt="pinout" width=800 height=550>


#### 24V passthrough
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_02.png" alt="pinout" width=800 height=550>


## :warning: Der Reihe nach folgende Schritte ausführen
* 1 CanBoot auf dem Rpi installieren
* 2 CanBoot Firmware erstellen und compilieren
* 3 CanBoot flashen
* 4 Klipper Firmware erstellen
* 5 can0 Schnittstelle auf dem Rpi konfigurieren
* 6 UUID vom EBB Board auslesen
* 7 EBB Klipper Firmware flashen
* 8 printer.cfg anpassen


## 1. CanBoot auf dem Rpi installieren

```
sudo apt-get install git -y
git clone https://github.com/Arksine/CanBoot
```

## 2.CanBoot Firmware erstellen und compilieren
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
 
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/F072_CanBoot.png" alt="CanBoot-F076">


#### BTT EBB36 v1.1 und v1.2 (Prozessor G0B1)

 - 8MHz crystal
 - Can bus on PB0/PB1 
 - 8KiB offset
 - 500000 CAN bus speed

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/G0B1_CanBoot.png" alt="CanBoot-G0B1">




## 3. CanBoot Bootloader flashen

### Vorbereitungen

### 1. 5V Jumper setzen
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB_DFU.png" alt="G0B1">

### 2. BTT EBB Board über das migelieferte USB Kabel mit dem PC verbinden

### 3. DFU Modus aktivieren

* Boot Taster drücken und gedrückt halten
* zusätzlich den Reset Taster Drücken und halten
* beide Taster wieder loslassen

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB_DFU_Modus.png" alt="G0B1">


## 4. Klipper Firmware erstellen
```
cd ~/klipper
make clean
make menuconfig
```

###### :warning: Hier bitte aufpassen, welches EBB36/42 Board verwendet wird und folgende Einstellungen verweden

#### BTT EBB36 v1.0 (Prozessor F072)

 - 8MHz crystal
 - CAN bus on PB8/PB9 
 - 8KiB offset
 - 500000 CAN bus speed
 
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/Klipper_F072.png" alt="Klipper-F072">

#### BTT EBB36 v1.1 und v1.2 (Prozessor G0B1)

 - 8MHz crystal
 - Can bus on PB0/PB1 
 - 8KiB offset
 - 500000 CAN bus speed
 
 <img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/Klipper_G0B1.png" alt="Klipper-G0B1">


## 5. can0 Schnittstelle auf dem Rpi konfigurieren
```
cd
sudo nano /etc/network/interfaces.d/can0
```
folgenden Inhalt kopieren und mit der rechten Maustaste in Putty einfügen
```
allow-hotplug can0
iface can0 can static
 bitrate 500000
 up ifconfig $IFACE txqueuelen 128
 pre-up ip link set can0 type can bitrate 500000
 pre-up ip link set can0 txqueuelen 128
```
##### mit STRG+X beenden und mit Y bestätigen


<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/can0.png" alt="can0">


## 6. UUID vom EBB Board auslesen
```
cd ~/CanBoot/scripts
pip3 install pyserial
ifconfig
python3 flash_can.py -i can0 -q
```


## 7. EBB Klipper Firmware flashen
```
python3 flash_can.py -f ~/klipper/ebb_klipper.bin -u <ebb_uuid>
```
du zuvor ausgelesene UUID einsprechend in den Befehl einsetzen

```
python3 flash_can.py -f ~/klipper/out/klipper.bin -u 330a31adf6de
```


## 8. printer.cfg anpassen

Folgende Punkte müsst ihr in eurer printer.cfg anpassen

- EBB MCU einfügen 
- Pin Belegung vom Extruder Motor 
- Pin Belegung vom Heater 
- Pin Belegung vom Hotend Thermistor
- Pin Belegung des Probe
- Pin Belegung Fan
- Pin Belgung Hotend Fan
- (Optional) Pin Belegung Neopixel Stealthburner LEDs
- (Optional) Temperatur vom EBB Board 
- (Optional) Pin Belegung vom adxl345 Sensor  
- (Optional) Pin Belegung X Endschalter, wenn der X Endschalter am Druckkopf montiert wurde

### Pinout BTT EBB36 v1.0

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB36%20CAN%20V1.0-PIN.png" alt="pinout-v1">

### Pinout BTT EBB36 v1.1 / v1.2

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB36%20CAN%20V1.1%26V1.2-PIN.png" alt="pinout-v1.1-v1.2">

Pinouts der EBB42 Versionen findet ihr hier: - [BTT EBB36/42 Github](https://github.com/bigtreetech/EBB)


## Bespiel für die Anpassung der Pins mit der neuen mcu EBB
###### alle anderen Einstellungen bleiben bestehen
```
[mcu EBB]
canbus_uuid: 330a31adf6de

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

[extruder]
step_pin: EBB:PD0
dir_pin: !EBB:PD1
enable_pin: !EBB:PD2
heater_pin: EBB:PA2
sensor_pin: EBB:PA3

[tmc2209 extruder]
uart_pin: EBB:PA15

[probe]
pin: EBB:PB8 

[fan]
pin: EBB:PA0

[heater_fan hotend_fan]
pin: EBB:PA1 

[temperature_sensor ebb_temp]
sensor_type: temperature_mcu
sensor_mcu: EBB
min_temp: 0
max_temp: 120
```



### Quellen: 
- [BTT U2C Github](https://github.com/bigtreetech/U2C/tree/master/firmware)
- [maz0r Github](https://maz0r.github.io/klipper_canbus/controller/firmware_files/utoc_firmware.bin)



