# CanBus Installation 


#### Support
Wenn ihr mir einen Kaffee ausgeben wollt: 
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/dfoure?country.x=DE&locale.x=de_DE)


### Verwendete Hardware 

 * BTT Octopus V1.1
 * BTT EBB36
 * BTT U2C V1.1

###### Es sind aber auch andere Hardware-Konstellationen (Fysetc Spider, Fly UTOC-1 / UTOC-3 möglich)
 
 
## U2C Firmware
Die U2C Board werden bereits mit einer passenden Firmware ausgeliefert.
Optional kann die Candlelight Firmware geflasht werden. 
Candlelight werde ich später noch ergännzen.

Für die BTT Boards findet ihr die original Firmware hier: [BTT U2C Firmware](https://github.com/bigtreetech/U2C/tree/master/firmware)

:warning: achtet auf den jeweiligen Chipsatz

Der Flashvorgang mittels STM32CubePorgrammer ist analog zum CanBoot Flash, das Vorgehen ist hier beschrieben.

DFU Modus aktivieren
- Board spannungslos schalten
- Boot Taster drücken und gedrückt halten
- USB Verbindung mit dem PC herstellen
- Boot Taster loslassen
 
## Vorbereitungen
#### Flasht euer Board ganz normal wie in der Voron Anleitung angegeben und bindet es als MCU mit der Serial in die printer.cfg ein
- [Voron Design](https://docs.vorondesign.com/build/software/#firmware-flashing)
#### STM32CubeProgrammer herunterladen und installieren
- [BTT Github](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master/Firmware/DFU%20Update%20bootloader/install%20software)
#### FTP Programm herunterladen und installieren (ich nutze WinSCP)
- [WinSCP](https://winscp.net/eng/download.php)


## Elektrischer Anschluss
Hier gibt es diverse Möglichkeiten. 
Bewährt haben sich bei mir folgende:

###### :warning: Aufgrund der Übersicht, wurde die 24V Verdrahtung vom Octopus nicht dargestellt. Diese ist nach der Voron Anleitung durchzuführen

#### without 24V passthrough
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_01.png" alt="pinout" width=800 height=550>


#### 24V passthrough
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_02.png" alt="pinout" width=800 height=550>


# :warning: Der Reihe nach folgende Schritte ausführen
* [CanBoot auf dem Rpi installieren](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#canboot-auf-dem-rpi-installieren)
* [CanBoot Firmware erstellen und compilieren](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#canboot-firmware-erstellen-und-compilieren)
* [CanBoot flashen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#canboot-bootloader-flashen)
* [Klipper Firmware erstellen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#4-klipper-firmware-erstellen)
* can0 Schnittstelle auf dem Rpi konfigurieren
* UUID vom EBB Board auslesen
* EBB Klipper Firmware flashen
* printer.cfg anpassen


# CanBoot auf dem Rpi installieren

```
sudo apt-get install git -y
git clone https://github.com/Arksine/CanBoot
```

# CanBoot Firmware erstellen und kompilieren
```
cd CanBoot
make menuconfig
make
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



# CanBoot Bootloader flashen

## :warning: Falls ihr die Heizpatrone bereits am EBB angeschlossen habt, entfernt diese vorerst, da es bei manchen Versionen vorkommt, dass der Heater Ausgang mit 100% angesteuert wird!

### :warning: Für diesen Schritt benötigen wir nur den USB Anschluss. Entfernt, falls bereits angeschlossen, den Molex Stecker mit der 24V Versorgung vom EBB

### canboot.bin Datei auf den Rechner kopieren

Öffnet WinSCP, loggt euch mit der IP vom RPi und euren Anmeldedaten ein.
Navigiert zum Ordner ```/home/pi/CanBoot/out``` und zieht die Datei ```canboot.bin``` via Drag&Drop auf euren Desktop oder in einen angelegten Ordner.

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/winscp.png" alt="WinSCP">

### 5V Jumper setzen
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB_DFU.png" alt="G0B1">

### BTT EBB Board über das mitgelieferte USB Kabel mit dem PC verbinden

### DFU Modus aktivieren

* Boot Taster drücken und gedrückt halten
* zusätzlich den Reset Taster Drücken und halten
* beide Taster wieder loslassen

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB_DFU_Modus.png" alt="G0B1">

### STM32CubeProgrammer starten 

#### mit dem Board verbinden

- USB Schnittstelle auswählen
- verbinden

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/STM_USB_connect.png" alt="connect">

#### Full Chip erase ausführen

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/STM_fullchiperase.png" alt="erase">

#### canboot.bin flashen

- Open File
- canboot.bin auswählen
- downloaden

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/STM_Download.png" alt="flashen">

#### Disconnecten
- Verbindung trennen
- Programm schließen

### 5V Jumper entfernen und 120Ohm Jumper setzen

- USB Verbindung zum PC trennen
- entfernt den 5V Jumper und setzt ihn an die Stelle für den 120Ohm Abschlusswiderstand

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/bootjumper.png" alt="120">

### Molexstecker wieder anschließen und das Board mit 24V versorgen


# Klipper Firmware erstellen
```
cd ~/klipper
make clean
make menuconfig
make
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


# 5. can0 Schnittstelle auf dem Rpi konfigurieren
```
cd
sudo nano /etc/network/interfaces.d/can0
```
folgenden Inhalt kopieren und mit der rechten Maustaste in Putty einfügen
```
allow-hotplug can0
iface can0 can static
 bitrate 500000
 up ifconfig $IFACE txqueuelen 256
 pre-up ip link set can0 type can bitrate 500000
 pre-up ip link set can0 txqueuelen 256
```
##### mit STRG+X beenden und mit Y bestätigen


<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/can0.png" alt="can0">


# 6. UUID vom EBB Board auslesen

can0 Netzwerk checken
```
ifconfig
```
Es sollte nun eine can0 Schnittstelle bei euch auftauchen

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/ifconfig_can0.png" alt="can0">

Weiter gehts mit folgenden Befehlen in Putty

```
cd ~/CanBoot/scripts
pip3 install pyserial
python3 flash_can.py -i can0 -q
```
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/canboot_nodes.png" alt="can0">

#### sollte es zu einem Fehler bei ```pip3 install pyserial``` kommen, versucht vorher folgenden Befehl auszuführen:

```
apt-get install python3-pip
```

Notiert euch die angezeigte UUID, diese wird im nächste Step benötigt.


# EBB Klipper Firmware flashen

Wir benötigen folgenden Befehl um das EBB zu flashen.

```
python3 flash_can.py -f ~/klipper/ebb_klipper.bin -u <ebb_uuid>
```

<ebb_uuid> muss mit der UUID vom EBB Board ausgetauscht werden

Als Beispiel lautet der Befehl mit einer meiner UUIDs:

```
python3 flash_can.py -f ~/klipper/out/klipper.bin -u 330a31adf6de
```
Der Flashvorhang sollte starten und erfolgreich abgeschlossen werden.

# printer.cfg anpassen

Folgende Punkte müsst ihr in eurer printer.cfg anpassen

- EBB MCU einfügen 
- Pin Belegung vom Extruder Motor 
- Pin Belegung vom Heater 
- Pin Belegung vom Hotend Thermistor
- Pin Belegung des Probe
- Pin Belegung Fan
- Pin Belegung Hotend Fan
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



