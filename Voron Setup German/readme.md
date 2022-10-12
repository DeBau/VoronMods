## Diese Anleitung basiert auf einem Voron V2 350er mit Octopus V1.1 Board ohne Mods

### Quellen: 
- [Voron Design Dokumentation](https://docs.vorondesign.com)
- [Voron Design Github Voron v2](https://docs.vorondesign.com)

## Vorbereitungen
### Download der Voron printer.cfg
- [Voron Design prnter.cfg für das Octopus Board](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus)

### Einstellungen in der printer.cfg die unbedingt überprüft werden müssen
- MCU ID     
- Thermistor Typen
- Z Endstop Position
- Homing end Position
- Z Endstop offset für Z0 
- Probe points   
- Min & Max gantry corner Positionen
- PID tune                     
- Probe pin  
- Fine tune E steps   

## Step by Step 

### printer.cfg vorbereiten
Ersteinmal kümmern wir uns um die Printer.cfg und kommentieren die entsprechenden Bereiche für unseren Drucker aus.
Alles was nicht benötigt wird, kann und sollte wegen der Übersichtlichkeit gelöscht werden.
Für einen Voron V2 350er sieht die fertige printer.cfg dann so aus:

- [Bespiel printer.cfg](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/printer.cfg)

Natürlich ist diese printer.cfg nicht allgemein gültig! 
Sie kann sich je nach verwendeten Motoren, Sensoren, Extruder, Board, Z-Endstop Position, Probe etc unterscheiden und sollte hier
nur der Veranschaulichung dienen! Bitte nicht 1:1 übernehmen

### Überprüfen der Temperaturen für das Hotend und Heizbett

Die angezeigten Temperaturen sollten im Ausgangszustand ungefähr die aktuelle Raumtemperatur anzeigen.
Ist das nicht der Fall, sollte der Seonsortyp und der Sensorpin überprüft werden
- [Sensortypen](https://www.klipper3d.org/Config_Reference.html?h=common+thermistors+thermistor#common-thermistors)

<img src="https://docs.vorondesign.com/build/startup/images/mainsail_temp_graph.png" alt="Temperaturen" width=600 height=400>

### Überprüfung der Heizelemente
Gebt einen Temperatur für das Heizbett und das Hotend von 50°C vor und beobachtet ob eure Temperatur steigt.
Wenn die Temperatur nicht steigen sollte, überprüft bitte den Anschluss und den Pin des jeweiligen Heizelementes

### PID Tuning
An diesem Punkt mach ich personlich direkt das PID Tuning

Dafür füge ich mir in meine printer.cfg oder einer inkludierten macro.cfg folgende Makros ein.
Man sollte es sich ja einfach machen ;)

[PID Makros](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/PID_Tuning_Makros)

Die Makros kann ich jetzt über die Weboberfläche über die entsprechende Schaltfläche oder über die Eingabe der Befehle:
- PID_EXTRUDER
- PID_BED

in der Konsole ausführen.

Nach jedem Befehl und ausgeführtem PID Tuning die ermittelten Werte mit der Eingabe 
- SAVE_CONFIG

in der Konsole speichern

### Stepper Motoren überprüfen
Um die Motoren zu überprüfen kann man den Stepper_Buzz Befehl nutzen.
Dafür gebt folgende Befehl in die Konsole ein

STEPPER_BUZZ STEPPER=stepper_x

Der Motor sollte sich nun 10x je 1mm in die positive Richtung und wieder zurück zur Ausgangsposition bewegen.
Dies wiederholt ihr für alle Motoren

- STEPPER_BUZZ STEPPER=stepper_y
- STEPPER_BUZZ STEPPER=stepper_z
- STEPPER_BUZZ STEPPER=stepper_z1
- STEPPER_BUZZ STEPPER=stepper_z2
- STEPPER_BUZZ STEPPER=stepper_z3
- STEPPER_BUZZ STEPPER=stepper_extruder

Wenn sich der Schrittmotor überhaupt nicht bewegt, überprüft die Einstellungen "enable_pin" und "step_pin" für den Schrittmotor. Wenn sich der Schrittmotor bewegt, aber nicht in seine ursprüngliche Position zurückkehrt, überprüft die Einstellung "dir_pin". Wenn 
der Schrittmotor in eine falsche Richtung schwingt, deutet dies im Allgemeinen darauf hin, dass der "dir_pin" für die Achse invertiert werden muss. Fügt dazu in der Pinter.cfg ein '!' an den "dir_pin" an (oder entferntes, falls bereits eines vorhanden ist). 
Wenn sich der Motor deutlich mehr oder weniger als einen Millimeter bewegt, überprüft die Einstellung rotation_distance.

Anordung der Motoren
<img src="https://docs.vorondesign.com/build/startup/images/V2-motor-positions.png" alt="v2Motoren" width=400 height=400>

#### ACHTUNG
Mein Vorgehen sieht etwas anders aus
Ich nutze den force_move Befehl um die Motoren zu testen
Forcen bedeutet, dass ich die Motoren ohne vorher zu homen verfahren kann. Hierbei wirken auch keine Abschaltungen
durch Endschalter. Dies kann euren Drucker zerstören!
Bitte nur anwenden, wenn ihr wisst was ihr macht!

Befehle:
- FORCE_MOVE STEPPER=stepper_z DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_z0 DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_z1 DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_z2 DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_z3 DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_x DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_y DISTANCE=2 VELOCITY=5 [ACCEL=100]
- FORCE_MOVE STEPPER=stepper_extruder DISTANCE=2 VELOCITY=5 [ACCEL=100]

Änderungen in der printer.cfg

[force_move]

enable_force_move: true

Fahrbewegungen bei den Befehlen:
- Alle Z Motoren sollten 2mm hoch fahren
- Force X - nach rechts und hinten
- Force Y - nach links und hinten





