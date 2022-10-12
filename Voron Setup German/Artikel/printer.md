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
- Multipliziere den Spitzenstrom mit 0,707, um den maximalen Strom  (RMS) zu ermitteln.

##### Beispiel

Der LDO 42STH130-1684 ist mit einem maximalen Strom von 1,68 Ampere spezifiziert. Der maximale Laufstrom beträgt 1,68 * .707 = 1,1877, abgerundet auf einen maximalen RMS von 1,1 Ampere. 

Unabhängig vom berechneten Maximalstrom des Motors beträgt der maximale Strom vom 2209-Treibers 1,2 Ampere. Die Berechnung bezieht sich also auf den Höchstwert, den der Motor verarbeiten kann. Es wird empfohlen, mit kleineren Werten zu beginnen und von dort aus zu arbeiten. Wenn Sie mit einem hohen Wert beginnen, kann dies zu geschmolzenen Motorhalterungen führen.

#### Welchen Modus für meine Treiber?

Standardmäßig setzt Klipper die TMC-Treiber in den "spreadCycle"-Modus. 
Wenn der Treiber "stealthChop" unterstützt, kann er durch Hinzufügen von stealthchop_threshold: 999999 in die TMC-Sektion in diesem Modus betrieben werden.

Im Allgemeinen bietet der SpreadCycle-Modus ein größeres Drehmoment und eine höhere Positionsgenauigkeit als der StealthChop-Modus. Allerdings kann der stealthChop-Modus bei einigen Druckern deutlich weniger hörbare Geräusche erzeugen.

Tests, bei denen die Modi verglichen wurden, haben gezeigt, dass sich die "Positionsverzögerung" bei Bewegungen mit konstanter Geschwindigkeit im stealthChop-Modus um etwa 75 % eines Vollschritts erhöht (bei einem Drucker mit 40 mm Rotationsabstand und 200 Schritten pro Umdrehung erhöhte sich die Positionsabweichung bei Bewegungen mit konstanter Geschwindigkeit beispielsweise um ~0,150 mm). Diese "Verzögerung beim Erreichen der angeforderten Position" kann sich jedoch nicht als signifikanter Druckfehler erweisen und man kann das ruhigere Verhalten des stealthChop-Modus vorziehen.

Es wird empfohlen, immer den Modus "spreadCycle" (ohne Angabe von stealthchop_threshold) oder immer den Modus "stealthChop" (mit stealthchop_threshold auf 999999) zu verwenden. Leider liefern die Treiber oft schlechte und verwirrende Ergebnisse, wenn der Modus geändert wird, während der Motor eine Geschwindigkeit ungleich Null hat.



