Bei Verwendung der Treibertypen 2208 / 2209 werden Spannung und Strom in der Software eingestellt. 
In Klipper haben die Motorströme zwei Einstellungen: 

- run_current 
- hold_current

Für die meisten Motoren ist es jedoch nicht mehr empfehlenswert, einen Haltestrom anzugeben. Die Stromeinstellungen in Klipper basieren auf dem Effektivwert 
(RMS) und nicht auf dem Spitzenstrom. In den technischen Datenblättern der meisten Motoren ist die Spitzenstromkapazität angegeben.

#### Berechnung der Ströme

Um die maximalen Strom für einen Stepper zu berechnen, geh wie folgt vor:
- Schau ins Datenblatt vom Schrittmotor
- Multipliziere den Spitzenstrom mit 0,707, um den maximalen Strom  (RMS) zu ermitteln.

##### Beispiel

Der LDO 42STH130-1684 ist mit einem maximalen Strom von 1,68 Ampere spezifiziert. Der maximale Laufstrom beträgt 1,68 * .707 = 1,1877, abgerundet auf einen maximalen RMS von 1,1 Ampere. 

Unabhängig vom berechneten Maximalstrom des Motors beträgt der maximale Strom vom 2209-Treibers 1,2 Ampere. Die Berechnung bezieht sich also auf den Höchstwert, den der Motor verarbeiten kann. Es wird empfohlen, mit kleineren Werten zu beginnen und von dort aus zu arbeiten. Wenn Sie mit einem hohen Wert beginnen, kann dies zu geschmolzenen Motorhalterungen führen.
