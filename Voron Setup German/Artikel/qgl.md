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

