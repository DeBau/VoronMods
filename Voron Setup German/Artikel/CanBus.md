### CanBus Installation 
folgende hardware wurde verwendet

 * BTT Octopus V1.1
 * BTT EBB36
 * Fly Mellow UTOC-3
 
 Das Octopus wie in der Voron Anleitung flashen
 https://docs.vorondesign.com/build/software/octopus_klipper.html
 


Der Reihe nach folgende Schritte ausführen
* 1. CanBoot auf dem Rpi installieren
* 2. CanBoot Firmware erstellen und compilieren
* 3. Klipper Firmware erstellen
* 4. can0 Schnittstelle auf dem Rpi konfigurieren
* 5. UUID vom EBB Board auslesen
* 6. EBB Klipper Firmware flashen
* 7. printer.cfg anpassen


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
Hier bitte aufpassen, welches EBB36 Board verwendet wird und folgende Einstellungen verweden

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


#### 3.Klipper Firmware erstellen
```
cd ~/klipper
make clean
make menuconfig
```

Auch hier wieder auf die Board Versionen achten

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


#### 4.can0 Schnittstelle auf dem Rpi konfigurieren
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


#### 5.UUID vom EBB Board auslesen
```
cd ~/CanBoot/scripts
pip3 install pyserial
ifconfig
python3 flash_can.py -i can0 -q
```


#### 6.EBB Klipper Firmware flashen
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


#### 7. printer.cfg anpassen
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

