### PID Tuning
Fügt folgende Makros in die printer.cfg oder einer inkludierten macro.cfg ein.

```
[gcode_macro PID_Extruder]
gcode:
  M106 S64
  PID_CALIBRATE HEATER=extruder TARGET=245

[gcode_macro PID_Bed]
gcode:
  PID_CALIBRATE HEATER=heater_bed TARGET=100
```
Die Makros können mit den Befehlen
```
PID_EXTRUDER
PID_BED
```
nacheinander ausgeführt werden. Dies dauert seine Zeit.

Nach jedem ausgeführten PID Tuning sind die ermittelten Werte zu speichern.
```
SAVE_CONFIG
```

