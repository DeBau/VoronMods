## Position vom Z-Endstop
Home zuerst wieder X und Y.
Fahre den Druckkopf in die Position, so dass sich die Düse genau über den Z-Endstop befindet.
Tippe 
```
M114 
```
in die Konsole und notiere dir die Werte für X und Y
Diese Werte müssen jetzt in der printer.cfg unter der Sektion [safe_z_home] eingetragen werden

Beispiel:
```
[safe_z_home]
home_xy_position:204,350
speed:100
z_hop:10
```

Speichere die printer.cfg und starte neu


Jetzt sollte der Drucker vollständig mit dem Befehl 
```
G28
```
homen.
