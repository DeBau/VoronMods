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

Wenn sich ein Metallobjekt in der Nähe des Messtasters befindet, sollte ```QUERY_PROBE``` ```"triggered"``` anzeigen. 

Wenn das Signal invertiert ist, fügt ein "!" vor der Pin-Definition hinzu.

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
G1 Z50 F600 (nur für den Fall, dass ihr durch den vorherigen Test noch sehr weit unten seid)
G1 X175 Y175 F3000
```
Anschließend tippe folgenden Befehl in die Konsole ein
```
PROBE_ACCURACY
```
Das Programm tastet das Bett 10 Mal hintereinander ab und gibt am Ende einen Wert für die Standardabweichung aus. 
Vergewisser dich, dass der gemessene Abstand keine Tendenz aufweist (allmählich ab- oder zunimmt während der 10 Messungen) und dass die Standardabweichung weniger als 0,003 mm beträgt.
