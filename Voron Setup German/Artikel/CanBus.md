# CanBus Installation 

#### :warning: wird aktuell nicht weiter gepflegt! 


#### Support
Wenn ihr mir einen Kaffee ausgeben wollt: 
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/dfoure?country.x=DE&locale.x=de_DE)


### Verwendete Hardware 

 * BTT Octopus V1.1
 * BTT EBB36 v1.1
 * BTT U2C 

###### Es sind aber auch andere Hardware-Konstellationen (Fysetc Spider, Fly UTOC-1 / UTOC-3 möglich)
 
## :warning: Vorbereitungen
 
<br>

<details>
    <summary>
        <b>       
Vorbereitungen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
#### Flasht euer Board ganz normal wie in der Voron Anleitung angegeben und bindet es als MCU mit der Serial in die printer.cfg ein
- [Voron Design](https://docs.vorondesign.com/build/software/#firmware-flashing)
#### STM32CubeProgrammer herunterladen und installieren
- [BTT Github](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master/Firmware/DFU%20Update%20bootloader/install%20software)
#### FTP Programm herunterladen und installieren (ich nutze WinSCP)
- [WinSCP](https://winscp.net/eng/download.php)
 
</details>
<br> 

## Halter / Mounts / Printheads  
</details>

<br>
<details>
    <summary>
        <b>       
        Eine kleine Auswahl an Mounts habe ich hier zusammengestellt (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
#### [Voron CB-C2 Mod](https://github.com/kejar31/VoronMods/tree/main/CB-C2?fbclid=IwAR2v2ZASvFZWxVNpFTgP3a7tUxhCuuK9y-F1k_sPwuQ7Z6dU4PnJrTDBL1Y)

VZ HextrudORT Mod für den CB-C2

#### [Voron CB-C2 Mod VZ HextrudORT](https://www.printables.com/de/model/369577-vz-hextrudort-for-stealthburner)


#### [Voron CB-C2 Mod VZ HextrudORT Door Fix](https://www.printables.com/de/model/381586-vz-hextrudort-for-stealthburner-door-fix-for-fly-s?fbclid=IwAR26nPBQW9ZnNcHr6nHcK-sSjfhzFGNH59jufnZYwh6Eb4tarDseuxNbFH0)

eine breite Auswahl an Mounts only für zu ziemlich alle Extruder

#### [KyaosMaker EBB Mounts](https://github.com/KayosMaker/CANboard_Mounts)

BTT U2C Din Rail Mount

#### [U2C Din Rail Mount](https://www.printables.com/de/model/266737-bigtreetecg-canbus-u2c-board-clip-mount-voron-24/files)

</details>
<br> 

<br>
<details>
    <summary>
        <b>       
        Voron V2 Z-Kette CanBus Mod (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
Z-Chain CanBus Mod
 
#### [Voron Can Bus z-chain move](https://www.printables.com/de/model/279739-voron-can-bus-z-chain-move)


 
 </details>
<br> 
 
 
## U2C Firmware / Terminierung
 
<br>
<details>
    <summary>
        <b>       
        BTT U2C (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>

### Firmware flashen -  BTT U2C v2
 
<br>
<details>
    <summary>
        <b>       
        Variante 1: Flashvorgang mittels STM32CubeProgrammer  (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
#### :warning: BTT u2c v2.1 - BTT Firmware Fix beachten und unbedingt flashen

Für die BTT Boards findet ihr die original Firmware hier: [BTT U2C Firmware](https://github.com/bigtreetech/U2C/tree/master/firmware)

#### Der Flashvorgang mittels STM32CubeProgrammer ist analog zum CanBoot Flash, das Vorgehen ist unter Step 3 beschrieben.

DFU Modus aktivieren
- Board spannungslos schalten / USB kabel entfernen
- Boot Taster drücken und gedrückt halten
- USB Verbindung mit dem PC herstellen
- Boot Taster loslassen

 </details>
<br>
 
<br>
<details>
    <summary>
        <b>       
        Variante 2: Mittels RPi und U2C im DFU Modus flashen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>

[Firmware](https://github.com/Arksine/CanBoot/files/10410265/G0B1_U2C_V2.zip)

- Firmware herunterladen und entpacken
- die Datei G0B1_U2C_V2.bin in das Verzeichnis /home/pi/ auf dem RPi kopieren

Dies könnt ihr auf verschiedene Arten machen, u.a:
- WinSCP
- Cyberduck (MAC)
- Windows Powershell über den Befehl: scp G0B1_U2C_V2.bi pi@IP-EURES-RPIs:/home/pi/

DFU Modus vom BTT U2C aktivieren
- Board spannungslos schalten / USB kabel entfernen
- Boot Taster drücken und gedrückt halten
- USB Verbindung mit dem PC herstellen
- Boot Taster loslassen

Um zu überprüfen ob ihr den DFU Modus aktiviert habt, gebt folgenden Befehl in Putty ein:

```
dfu-util -l
```

Es sollte: Found DFU: [Eure Device ID] angezeigt werden

Weiter gehts mit dem FW-Flash [Putty]:

```
dfu-util -D ~/G0B1_U2C_V2.bin -a 0 -s 0x08000000:leave
```

error during download get-status könnt ihr ignorieren.

- Board spannungslos schalten / USB kabel entfernen
- USB Verbindung mit der RPi wieder herstellen
 
</details>
<br> 

### Terminierung / Abschlusswiderstand beim BTT U2C
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/U2C_resistor.png" alt="resistor">

Es geht auch ohne die Widerstände am U2C, aber schaut man sich mal folgende Grafik an, sieht man deutlich was eine korrekte Terminierung ausmacht

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/Can_term.png" alt="terminirung">

### BTT U2C PinOut

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/U2c_pinout.png" alt="resistor">

</details>
<br> 

<br>
<details>
    <summary>
        <b>       
        Fly SHT UTOC 1-3 (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 folgt! 
 </details>
<br> 
 
 
## Elektrischer Anschluss

<br>
<details>
    <summary>
        <b>       
        Elektrischer Anschluss (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 folgt! 
 </details>
<br> 

Hier gibt es diverse Möglichkeiten. 
Bewährt haben sich bei mir folgende:

:warning: Arbeiten sollten von einer Elektrofachkraft ausgeführt werden

Bitte unbedingt vorher den Netzstecker ziehen

Beachtet, auch wenn es nur ein 3D Drucker ist, die ersten 3 Regeln der 5 Sicherheitsregeln:

    - Freischalten
    - Gegen Wiedereinschalten sichern
    - Spannungsfreiheit feststellen

<br>
<details>
    <summary>
        <b>       
        without 24V passthrough (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
###### :warning: Aufgrund der Übersicht, wurde die 24V Verdrahtung vom Octopus nicht dargestellt. Diese ist nach der Voron Anleitung durchzuführen
 
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanBus001.png" alt="Can01">
 </details>
<br>

<br>
<details>
    <summary>
        <b>       
        24V passthrough (Bitte aufklappen)
        </b>
    </summary>
 
 ###### :warning: Aufgrund der Übersicht, wurde die 24V Verdrahtung vom Octopus nicht dargestellt. Diese ist nach der Voron Anleitung durchzuführen
 
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/CanSetup_02.png" alt="Can02">
 
 </details>
<br>

<br>
<details>
    <summary>
        <b>       
        :warning: Steckerbelegung (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>

Bitte prüft auf der Rückseite der EBB und U2C Board`s die korrekte Steckerbelegung.

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB_u2c_wiring.jpg" alt="wiring">

 </details>
<br>

<br>
<details>
    <summary>
        <b>       
        CANBus Leitungen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 

den richtigen Typ habe ich für den Anwendungsfall noch nicht wirklich gefunden. Falls jemand eine gute Quelle hat, schreibt mich bitte via Discord an: deba#0887

Bei der Auswahl sollte man sich an folgenden Punkten orientieren:

- benötigte Leistung (Heater + EBB Board)
- Leitungslänge (tatsächliche Länge *2)
- Material 
- Verlegeart
- Strombelastbarkeit (diese sinkt bei Bauraumtemperaturen von 50°C drastisch!)
- Spannungsfall (<3%)
- Temperaturbereich
- CANBus Spezifakationen
- für dauerhafte Bewegungen ausgelegt

Aktuell eingesetzt werden von mir:

- Ölflex Classic 810 4x0.75mm² / 4x1mm²
- Twisted Pair (YSTY) als Datenleitung + Einzeladern 0,75mm²-1mm² für die Spannungsversorgung
</details>
<br>
 
</details>
<br> 
                      
                      
# :warning: Der Reihe nach folgende Schritte ausführen
* [CanBoot auf dem Rpi installieren](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-1-canboot-auf-dem-rpi-installieren)
* [CanBoot Firmware erstellen und compilieren](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-2-canboot-firmware-erstellen-und-kompilieren)
* [CanBoot flashen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-3-canboot-bootloader-flashen)
* [Klipper Firmware erstellen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-4-klipper-firmware-erstellen)
* [can0 Schnittstelle auf dem Rpi konfigurieren](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-5-can0-schnittstelle-auf-dem-rpi-konfigurieren)
* [UUID vom EBB Board auslesen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-6-uuid-vom-ebb-board-auslesen)
* [EBB Klipper Firmware flashen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-7-ebb-klipper-firmware-flashen)
* [printer.cfg anpassen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#step-8-printercfg-anpassen)




## Step 1: CanBoot auf dem Rpi installieren

<br>
<details>
    <summary>
        <b>       
CanBoot auf dem Rpi installieren (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
#### :warning: System aktualisieren
System updaten und Python installieren
 
```
sudo apt update
sudo apt upgrade
sudo apt install python3 python3-pip python3-can
pip3 install pyserial
```

Sollten bei euch, nach dem Update, keine USB Verbindungen mehr möglich sein, führt folgenden FIX aus:
 
```
sudo cp /usr/lib/udev/rules.d/60-serial.rules /usr/lib/udev/rules.d/60-serial.old
```
```
sudo wget -O /usr/lib/udev/rules.d/60-serial.rules https://raw.githubusercontent.com/systemd/systemd/main/rules.d/60-serial.rules
```
```
sudo reboot
```

CanBoot installieren:

```
sudo apt-get install git -y
git clone https://github.com/Arksine/CanBoot
```
</details>
<br>
 

 
## Step 2: CanBoot Firmware erstellen und kompilieren

<br>
<details>
    <summary>
        <b>       
CanBoot Firmware erstellen und kompilieren (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
Putty: 

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

- mit der Taset "Q" beenden 
- und mit der Taste "Y" bestätigen


Putty:

```
make clean
make
```
</details>
<br>
 
 
## Step 3: CanBoot Bootloader flashen

<br>
<details>
    <summary>
        <b>       
CanBoot Bootloader flashen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
## :warning: Falls ihr die Heizpatrone bereits am EBB angeschlossen habt, entfernt diese vorerst, da es bei manchen Versionen vorkommt, dass der Heater Ausgang mit 100% angesteuert wird!

### :warning: Für diesen Schritt benötigen wir nur den USB Anschluss. Entfernt, falls bereits angeschlossen, den Molex Stecker mit der 24V Versorgung vom EBB

<br>
<details>
    <summary>
        <b>       
        Möglichkeit 1: CanBoot mittels STM32CubeProgrammer flashen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>


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

- USB Verbindung trennen
- entfernt den 5V Jumper und setzt ihn an die Stelle für den 120Ohm Abschlusswiderstand

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/bootjumper.png" alt="120">

### Molexstecker wieder anschließen und das Board mit 24V versorgen

</details>
<br> 

<br>
<details>
    <summary>
        <b>       
        Möglichkeit 2: im DFU Modus mittels Rpi flashen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>



### 5V Jumper setzen
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB_DFU.png" alt="G0B1">

### DFU Modus am EBB36 aktivieren

* EBB36 mit dem RPi verbinden
* Boot Taster drücken und gedrückt halten
* zusätzlich den Reset Taster Drücken und halten
* beide Taster wieder loslassen

in Putty folgenden Befehl ausführen um eure Device ID auszulesen
 ```
lsusb
```
Ihr erhaltet als Ausgabe eine Auflistung der angeschlossenen USB Geräte, wobei uns hier nur das EBB36 im DFU Modus interessiert.

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/lusb_device_id.png" alt="device_id">


0483:df11 ist die Device ID, die wir im nächsten Step benötigen (eure könnte anders sein)

Den Flashvorgang startet ihr mit folgendem Befehl: 

 ``` 
sudo dfu-util -a 0 -D ~/CanBoot/out/canboot.bin --dfuse-address 0x08000000:force:mass-erase:leave -d <device_id>
 ``` 
 
:warning: <device_id> entsprechend durch die gerade ermittelte ersetzen

 ``` 
sudo dfu-util -a 0 -D ~/CanBoot/out/canboot.bin --dfuse-address 0x08000000:force:mass-erase:leave -d 0483:df11
 ``` 
 
 ### 5V Jumper entfernen und 120Ohm Jumper setzen

- USB Verbindung trennen
- entfernt den 5V Jumper und setzt ihn an die Stelle für den 120Ohm Abschlusswiderstand

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/bootjumper.png" alt="120">

### Molexstecker wieder anschließen und das Board mit 24V versorgen

</details>
<br>

</details>
<br>
 
## Step 4: Klipper Firmware erstellen

<br>
<details>
    <summary>
        <b>       
        Klipper Firmware erstellen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
 
Putty:

```
cd ~/klipper
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
 
 - mit der Taset "Q" beenden 
 - und mit der Taste "Y" bestätigen


Abschließend folgende Befehle ausführen:

```
make clean
make
```
</details>
<br>
 
## Step 5: can0 Schnittstelle auf dem Rpi konfigurieren
 
<br>
<details>
    <summary>
        <b>       
can0 Schnittstelle auf dem Rpi konfigurieren (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
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
##### mit STRG+X beenden und mit Y und Enter bestätigen


<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/can0.png" alt="can0">

Abschließend einen Reboot vom RPi ausführen

```
sudo reboot
```
</details>
<br>
 
## Step 6: UUID vom EBB Board auslesen

 <br>
<details>
    <summary>
        <b>       
UUID vom EBB Board auslesen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
can0 Netzwerk checken
```
ifconfig
```
Es sollte nun eine can0 Schnittstelle bei euch auftauchen

<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/ifconfig_can0.png" alt="can0">

Weiter gehts mit folgenden Befehlen in Putty

```
cd ~/CanBoot/scripts
python3 flash_can.py -i can0 -q
```
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/canboot_nodes.png" alt="can0">

Notiert euch die angezeigte UUID, diese wird im nächste Step benötigt.

 
 
:warning: Was mache ich, wenn mir die UUID nicht angezeigt wird? 

Überprüft folgende Punkte: [Fehlersuche](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#fehlersuche)

</details>
<br>
 

## Step 7: EBB Klipper Firmware flashen
 
<br>
<details>
    <summary>
        <b>       
EBB Klipper Firmware flashen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>

Wir benötigen folgenden Befehl um das EBB zu flashen.

```
python3 flash_can.py -f ~/klipper/out/klipper.bin -u <ebb_uuid>
```

<ebb_uuid> muss mit der UUID vom EBB Board ausgetauscht werden

Als Beispiel lautet der Befehl mit einer meiner UUIDs:

```
python3 flash_can.py -f ~/klipper/out/klipper.bin -u 330a31adf6de
```
Der Flashvorhang sollte starten und erfolgreich abgeschlossen werden.

</details>
<br>
 
 
## Step 8: printer.cfg anpassen
 
<br>
<details>
    <summary>
        <b>       
printer.cfg anpassen (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>

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

<br>
<details>
    <summary>
        <b>       
        Pinout BTT EBB36 v1.0 (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB36%20CAN%20V1.0-PIN.png" alt="pinout-v1">
 </details>
<br>
<br>
<details>
    <summary>
        <b>       
        Pinout BTT EBB36 v1.1 / v1.2 (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB36%20CAN%20V1.1%26V1.2-PIN.png" alt="pinout-v1.1-v1.2">
</details>
<br>
 
Pinouts der EBB42 Versionen findet ihr hier: - [BTT EBB36/42 Github](https://github.com/bigtreetech/EBB)
 
</details>
<br>
 
<br>
<details>
    <summary>
        <b>       
Bespiel für die Anpassung der Pins mit der neuen mcu EBB (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 

### [Beispiel printer.cfg (klick)](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/v2_CANbus_printer_cfg.md)
```
Übersicht der anzupassenden Pins

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

#### Optional für die Voron Stealthburner LEDs

Die Datei stealthburner_leds.cfg herunterladen, als Datei unter Mainsail einfügen und die entsprechende Pin Belegung wählen

#### [Stealthburner LED CANbus](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/stealthburner_leds.cfg)

in der printer.cfg folgenden Zeile hinzufügen:

```
[include stealthburner_leds.cfg]
```

</details>
<br>
 
 
 
 
 
 
# Rapido Plus HF/UHF Anschluss und Konfiguration 

<br>
<details>
    <summary>
        <b>       
        EBB36 mit MAX31865 (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/Rapido_Plus_EBB_Max.png" alt="Rapiode_EBBMax">
 
- PT1000 am Port P3 Pin 2/3 anschließen. Die Polung ist hierbei nicht entscheidend.
- DIP Schalter passend stellen
- Extruder Sektion entsprechend anpassen
 
```
[extruder]
sensor_type:MAX31865
sensor_pin: EBB:PA4
spi_bus: spi1
rtd_nominal_r: 1000
rtd_reference_r: 4300
rtd_num_of_wires: 2
rtd_use_50Hz_filter: True
```
 </details>
<br>
 
<br>
<details>
    <summary>
        <b>       
        EBB36 ohne MAX31865 (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/Rapido_Plus_EBB.png" alt="Rapiode_EBB">
 
- PT1000 am Port TH0 anschließen. Die Polung ist hierbei nicht entscheidend.
- Jumper setzen
- Extruder Sektion entsprechend anpassen

```
[extruder]
sensor_type: PT1000
sensor_pin: EBBCan: PA3
pullup_resistor: 2200
```
  </details>
<br>
 
# Anschluss Extruder Motor LDO 36STH20-1004AHG(XH)
<br>
<details>
    <summary>
        <b>       
        EBB36 v1.1/v1.2 mit LDO 36STH20-1004AHG(XH) (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus_Pics/EBB36_E0.png" alt="EBB36_E0"> 
 
 Die Farben Rot/Grün und Gelb/Blau dürfen untereinander getauscht werden.
 
 Sollte der Extruder falsch herum drehen muss in der printer.cfg --> Sektion [extruder] der DIR Pin negiert werden
 
 ```
Bespiel
aus:
 
[extruder]
dir_pin: !EBB:PD1

wird folgendes:
 
[extruder]
dir_pin: EBB:PD1
```
   </details>
<br>
 
 
 # Fehlersuche
 
 <br>
<details>
    <summary>
        <b>       
        Häufige Fehler bei der Einrichtung (Bitte aufklappen)
        </b>
    </summary>
<p>
</p>
 
### UUID kann nicht ausgelesen werden
 
#### Unterschiedliche Baudraten 
 
Achtet bitte darauf überall 500000 einzustellen.
Mischen der Baudraten ist hier nicht möglich.
Es gibt Anleitungen die mit 250000 arbeiten. Wenn ihr dies auch wollt, stellt bitte auch überall 250000 ein

#### EBB36 v1.1/v1.2 Communication interfaces 
 
Achtet bitte genau auf die Pins des Communication interfaces. 
(Can bus (on PB0/PB1)) muss eingestellt werden.
(Can bus (on PD0/PD1)) gibt es auch, wird aber so nicht funktionieren
 
#### Can High/Low Leitungen vertauscht
 
Vergewissert euch, dass die Leitungen korrekt angeschlossen sind.
 
#### BTT U2C V2
Probiert den Firmware Fix zu flashen: [BTT U2C FW Fix](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md#u2c-firmware--terminierung)

 
### Verbindung bricht während des Drucken ab
 
#### Schlecht gecrimpte Leitungen 
 
Bei sporadischen Ausfällen während der Druckbewegung, kann es am Stecker, bzw. an der Crimps der
CanBus Leitung liegen. 
Überprüft diese auf festen Sitz und ordentlichen Kontakt
 
 
   </details>
<br>
 
 
### Quellen und Nachschlagewerke: 
- [Arkshine Canboot](https://github.com/Arksine/CanBoot)
- [Klipper CANBus](https://www.klipper3d.org/CANBUS.html)
- [BTT U2C Github](https://github.com/bigtreetech/U2C/tree/master/firmware)
- [maz0r Github](https://github.com/maz0r/klipper_canbus/blob/main/index.md)
- [chripink](https://github.com/chripink/CanBus-Tuto/blob/4b70f8c3235076c44313e267ab36b370b07d1f80/README_EN.md)
- [BTT Github](https://github.com/bigtreetech/EBB)



