### Stepper Motoren überprüfen
Um die Motoren zu überprüfen kann man den Stepper_Buzz Befehl nutzen.
Dafür gebt folgenden Befehl in die Konsole ein
```
STEPPER_BUZZ STEPPER=stepper_x
```
Der Motor sollte sich nun 10x je 1mm in die positive Richtung und wieder zurück zur Ausgangsposition bewegen.
Dies wiederholt ihr für alle Motoren
```
STEPPER_BUZZ STEPPER=stepper_y
STEPPER_BUZZ STEPPER=stepper_z
STEPPER_BUZZ STEPPER=stepper_z1
STEPPER_BUZZ STEPPER=stepper_z2
STEPPER_BUZZ STEPPER=stepper_z3
STEPPER_BUZZ STEPPER=stepper_extruder
```
Wenn sich der Schrittmotor überhaupt nicht bewegt, überprüft die Einstellungen "enable_pin" und "step_pin" für den Schrittmotor. Wenn sich der Schrittmotor bewegt, aber nicht in seine ursprüngliche Position zurückkehrt, überprüft die Einstellung "dir_pin". Wenn 
der Schrittmotor in eine falsche Richtung schwingt, deutet dies im Allgemeinen darauf hin, dass der "dir_pin" für die Achse invertiert werden muss. Fügt dazu in der Pinter.cfg ein '!' an den "dir_pin" an (oder entferntes, falls bereits eines vorhanden ist). 
Wenn sich der Motor deutlich mehr oder weniger als einen Millimeter bewegt, überprüft die Einstellung rotation_distance.

#### Anordung der Motoren
<img src="https://docs.vorondesign.com/build/startup/images/V2-motor-positions.png" alt="v2Motoren" width=400 height=400>

#### ACHTUNG
Mein Vorgehen sieht etwas anders aus
Ich nutze den force_move Befehl um die Motoren zu testen
Forcen bedeutet, dass ich die Motoren ohne vorher zu homen verfahren kann. Hierbei wirken auch keine Abschaltungen
durch Endschalter. Dies kann euren Drucker zerstören!
Bitte nur anwenden, wenn ihr wisst was ihr macht!

Befehle:
```
FORCE_MOVE STEPPER=stepper_z DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_z0 DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_z1 DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_z2 DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_z3 DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_x DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_y DISTANCE=2 VELOCITY=5 [ACCEL=100]
FORCE_MOVE STEPPER=stepper_extruder DISTANCE=2 VELOCITY=5 [ACCEL=100]
```
Wer sich mit den Force Move auskennt, wird wissen was er noch in die printer.cfg einfügen muss um die Befehle zum Laufen zu bekommen ;)
