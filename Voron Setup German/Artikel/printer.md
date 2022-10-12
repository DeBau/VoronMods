Ersteinmal kümmern wir uns um die Printer.cfg und kommentieren die entsprechenden Bereiche für unseren Drucker aus.
Alles was nicht benötigt wird, kann und sollte wegen der Übersichtlichkeit gelöscht werden.
Für einen Voron V2 350er sieht die fertige printer.cfg dann so aus:

- [Bespiel printer.cfg](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/printer.cfg)

Natürlich ist diese printer.cfg nicht allgemein gültig! 
Sie kann sich je nach verwendeten Motoren, Sensoren, Extruder, Board, Z-Endstop Position, Probe etc unterscheiden und sollte hier
nur der Veranschaulichung dienen! Bitte nicht 1:1 übernehmen

### Sensortypen

- EPCOS 100K B57560G104F"
- ATC Semitec 104GT-2",
- ATC Semitec 104NT-4-R025H42G
- Generic 3950
- Honeywell 100K 135-104LAG-J01
- NTC 100K MGB18-104F39050L32",
- SliceEngineering 450
- TDK NTCG104LH104JT1

### Berechnung von Motorstrom

Bei Verwendung der Treibertypen 2208 / 2209 werden Spannung und Strom in der Software eingestellt. 
In Klipper haben die Motorströme zwei Einstellungen: 

- run_current 
- hold_current

Für die meisten Motoren ist es jedoch nicht mehr empfehlenswert, einen Haltestrom anzugeben. Die Stromeinstellungen in Klipper basieren auf dem Effektivwert (RMS) und nicht auf dem Spitzenstrom. In den technischen Datenblättern der meisten Motoren ist die Spitzenstromkapazität angegeben.

#### Berechnung der Ströme

Um die maximalen Strom für einen Stepper zu berechnen, geh wie folgt vor:
 - Schau ins Datenblatt vom Schrittmotor
- Multipliziere den Spitzenstrom mit 0,707, um den maximalen Strom in RMS zu ermitteln.

##### Beispiel

Der LDO 42STH130-1684 ist mit einem maximalen Strom von 1,68 Ampere spezifiziert. Der maximale Laufstrom beträgt 1,68 * .707 = 1,1877, abgerundet auf einen maximalen RMS-Laufstrom von 1,1 Ampere. 

Unabhängig vom berechneten Maximalstrom des Motors beträgt der maximale Strom vom 2209-Treibers 1,2 Ampere. Die Berechnung bezieht sich also auf den Höchstwert, den der Motor verarbeiten kann. Es wird empfohlen, mit kleineren Werten zu beginnen und von dort aus zu arbeiten. Wenn Sie mit einem hohen Wert beginnen, kann dies zu geschmolzenen Motorhalterungen führen.
