### PID Tuning
An diesem Punkt mach ich personlich direkt das PID Tuning

Dafür füge ich mir in meine printer.cfg oder einer inkludierten macro.cfg folgende Makros ein.
Man sollte es sich ja einfach machen ;)

[PID Makros](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/PID_Tuning_Makros)

Die Makros kann ich jetzt über die Weboberfläche über die entsprechenden Schaltflächen oder über die Eingabe der Befehle:
```
PID_EXTRUDER
PID_BED
```
in der Konsole ausführen.

Nach jedem Befehl und ausgeführtem PID Tuning die ermittelten Werte mit der Eingabe 
```
SAVE_CONFIG
```
in der Konsole speichern
