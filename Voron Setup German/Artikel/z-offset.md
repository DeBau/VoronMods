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
Sollte ihr mal zu tief gefahren sein, ist es natürlich auch möglich mittels ```TESTZ``` hoch zu fahren.

```
TESTZ Z=0.1
TESTZ Z=0.25
etc...
```
