## Initial Setup für einen Voron V2 350
- Octopus v1.1 Board
- TMC2209 Treiber
- Moons Motoren (17HS19-2004S und 17HS08-1004S)
- Omron TL-Q5MC2-Z Sensor
- mini12864 LCD Display

### Quellen: 
- [Voron Design Dokumentation](https://docs.vorondesign.com)
- [Voron Design Github Voron v2](https://docs.vorondesign.com)
- [Klipper](https://www.klipper3d.org/)

#### Support
Wenn ihr mir einen Kaffee ausgeben wollt: 
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/dfoure?country.x=DE&locale.x=de_DE)


## Vorbereitungen
### Download der Voron printer.cfg
- [Voron Design printer.cfg für das Octopus Board](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus)
### PinOut vom Octopus Board
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/BIGTREETECH-Octopus-1.1-color-PIN.jpg" alt="pinout" width=800 height=400>

# Inhaltsverzeichnis
## Grundeintellungen
*Notwendige Anpassungen*
- [printer.cfg](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/printer.md) 
##### Infos rund um die printer.cfg und deren Einstellungen 
- [Motorstromberechnung](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Motorstrom.md)
- [TMC Treiber](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/TMC_Treiber.md)
## Temperaturen
- [Thermistoren und Heater](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Temperaturen.md) 
- [PID Tuning](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/PID_Tuning.md) 
## Motion
1. [Stepper überprüfen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Stepper.md) 
     #### Endstops überprüfen
          2. [XYZ](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/XYZ_Endstops.md) 
     #### Homen
          3. [XY Homen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/XY_Homen.md) 
     #### Z Position und Druckbettausrichtung
          4. [Druckbettausrichtung](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Ausrichtung.md) 
          5. [0-Punkt Bestimmung](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/0-Punkt.md) 
          6. [Z-Endstop Position festlegen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Z_Endstop.md) 
### Probe



## Quad Gantry Level

## Offsets







#





Sollten die Axen falsch fahren, muss evntuell der DIR-PIN von den AB Motoren negiert, oder die Anschlüsse der beiden Motoren getauscht werden. Hierfür ist folgende Grafik recht hilfreich.
Bitte niemals im Eingeschalteten Zustand eine Motorleitung vom Motor oder Mainboard entfernen! 

<img src="https://docs.vorondesign.com/build/startup/images/V2-motor-configuration-guide.png" alt="v2Motorenmove" width=700 height=400>


## 0,0-Punkt bestimmen



#
     
## Probe
Home den Drucker und fahre in die Mitte des Druckbettes in 50mm Höhe
```
G28
G90
G1 X175 Y175 F3000
G1 Z50 F600
```
Der Probe sollte nach Eingabe von folgenden Befehl:
```
QUERY_PROBE
```
"open" anzeigen. 
Wenn sich ein Metallobjekt in der Nähe des Messtasters befindet, sollte QUERY_PROBE "triggered" anzeigen. Wenn das Signal invertiert ist, fügt ein "!" vor der Pin-Definition hinzu.
Verringert nun langsam die Z-Höhe
```
G1 Z10 F600
G1 Z9 F600
G1 Z8 F600
usw.
```
und führt
```
QUERY_PROBE 
```
jedes Mal aus, bis QUERY_PROBE "getriggert" anzeigt.
Stellt sicher, dass die Düse die Druckoberfläche nicht berührt

### Wiederholgenauigkeit
fahr den Druckkopf mittig übers Bett
```
G90
G1 Z50 F600 (nur für den Fall, dass er durch den vorherigen Test noch sehr weit unten seid)
G1 X175 Y175 F3000
```
Anschließend tippe folgenden Befehl in die Konsole ein
```
PROBE_ACCURACY
```
Das Programm tastet das Bett 10 Mal hintereinander ab und gibt am Ende einen Wert für die Standardabweichung aus. 
Vergewisser dich, dass der gemessene Abstand keine Tendenz aufweist (allmählich ab- oder zunimmt während der 10 Messungen) und dass die Standardabweichung weniger als 0,003 mm beträgt.

## Quad Gantry Level 
Da der V2 4 unabhängige Z-Motoren verwendet, muss das gesamte Gantry-System speziell nivelliert werden. Das Makro, mit dem dieser Prozess aufgerufen wird, heißt QUAD_GANTRY_LEVEL (im Sprachgebrauch manchmal auch als 'QGL' bezeichnet). Es prüft jeden der 4 Punkte dreimal, ermittelt den Durchschnitt der Messwerte und nimmt dann Anpassungen vor, bis das Portal waagerecht ausgerichtet ist.
Wenn der Prozess aufgrund eines Fehlers "außerhalb der Grenzen" fehlschlägt, deaktiviert die Schrittmotoren und bewegt das Portal langsam von Hand, bis es annähernd gerade ist.
```
QUAD_GANTRY_LEVEL
```

### QGL im aufgeheiztem Zustand

Heize das Bett auf 100°C und die Nozzle auf 240°C, schließe die Türen und warte 15min.
Fahr in die Mitte
```
G28
G90
G1 X 175 Y175 F3000
```
Dies wird das erste Mal sein, dass ein Quad Gantry Level bei einer hohen Kammertemperatur ausgeführt wird. 
Führe den Befehl aus:
```
PROBE_ACCURACY
```
Wenn die Werte über die 10 Tastzyklen hinweg eine Tendenz aufweisen (steigend oder fallend) oder die Standardabweichung größer als 0,003 mm ist, warte weitere 5 Minuten und versuche es erneut.

Sobald die Messwerte stabil sind, führe
```
QUAD_GANTRY_LEVEL
```
aus. 
Notieren dir, wie lange die Messwerte des Sonsors brauchten, um sich zu stabilisieren - ein kalter Drucker braucht in der Regel eine 10-20 minütige Vorheizphase

## Z-Offset-Einstellung
Lasst den Drucker min 15min bei 245°C Hotend und 100°C Bett vorheizen

Vorbereitung (Befehle nacheinander in die Konsole eingeben)
```
G28
QUAD_GANTRY_LEVEL
G28
BED_MESH_CLEAR
G90
G1 X175 Y175 F3000
```
Anschließend folgenden Befehl ausführen
```phyton
Z_ENDSTOP_CALIBRATE
```
Legt ein Blatt Papier unter den Druckkopf aufs Bett und fahrt den Druckkopf langsam in Richtung Bett
```
TESTZ Z=-1 
```
bis die Düse relativ nahe am Bett ist.

Das "Feintuning" erledigt dann mit kleinerer Abstufung
```
TESTZ Z=-0.1
TESTZ Z=-0.025
TESTZ Z=-0.001
etc...
```
Wenn das Papier bei Bewegung leicht an der Nozzle kratzt, entfernt es und fahrt nochmals um die Dicke des Papieres runter
```
TESTZ Z=-0.1
```
Akzeptiert und speichert den neuen Z-Offset Wert mit folgenden Befehlen
```
ACCEPT 
SAVE_CONFIG 
```
Sollte ihr mal zu tief gefahren sein, ist es natürlich auch möglich mittel TESTZ hoch zu fahren.

```
TESTZ Z=0.1
TESTZ Z=0.25
etc...
```


