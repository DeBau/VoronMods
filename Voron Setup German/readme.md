## Initial Setup für einen Voron V2 350
- Octopus v1.1 Board
- TMC2209 Treiber
- Moons Motoren (17HS19-2004S und 17HS08-1004S)
- Omron TL-Q5MC2-Z Sensor
- mini12864 LCD Display

### Quellen: 
- [Voron Design Dokumentation](https://docs.vorondesign.com)
- [Voron Design Github Voron v2](https://docs.vorondesign.com)
- [Klipper](https://www.klipper3d.org/)

## Vorbereitungen
### Download der Voron printer.cfg
- [Voron Design prnter.cfg für das Octopus Board](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus)
### PinOut vom Octopus Board
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/BIGTREETECH-Octopus-1.1-color-PIN.jpg" alt="pinout" width=800 height=400>


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
Ist das nicht der Fall, sollte der Sensortyp und der Sensorpin überprüft werden
- [Sensortypen](https://www.klipper3d.org/Config_Reference.html?h=common+thermistors+thermistor#common-thermistors)

<img src="https://docs.vorondesign.com/build/startup/images/mainsail_temp_graph.png" alt="Temperaturen" width=600 height=400>

### Überprüfung der Heizelemente
Gebt eine Temperatur für das Heizbett und das Hotend von 50°C vor und beobachtet ob eure Temperatur steigt.
Wenn die Temperatur nicht steigen sollte, überprüft bitte den Anschluss und den Pin des jeweiligen Heizelementes.

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

### Endstops kontrollieren
Vergewisser dich, dass keiner der X-, Y- oder Z-Endstopps betätigt ist. 
Tippe dann
```
QUERY_ENDSTOPS 
```
in die Konsole ein.
Folgendes solltest du dann angezeigt bekommen:
```
x:open y:open z:open
```
Wenn ein Endschalter "triggered" statt "open" anzeigt, überprüfe bitte ob wirklich keiner betätigt ist. 
Jetzt kannst du den X-Endschalter händisch betätigen und erneut QUERY_ENDSTOPS in die Konsole eingeben.

Die anzeige sollte nun lauten:
```
x:triggered y:open z:open
```
Wiederhole den Test auch für den Y und Z Endschalter.

Wenn die Endschalter falsch herum arbeiten, sprich bei Nichtbetätigung schon "triggered" anzeigen, dann geh in die printer.cfg und füge ein ! vor der Pin ein, bzw. entferne es, wenn es schon vorhanden ist. 

als Beispiel:
```
aus endstop_pin: P1.28 wird endstop_pin: !P1.28
```

## XY-Homen

Wichtig! Hand auf den Netzschalter, oder mit der Maus in der Weboberfläche auf den Emergency Stop um bei einem Fehler schnell reagieren und den Drucker ausschalten zu können.

Homen von X mit dem Befehl
```
G28 X
```
Homen von Y mit dem Befehl
```
G28 Y
```

Sollten die Axen falsch fahren, muss evntuell der DIR-PIN von den AB Motoren negiert, oder die Anschlüsse der beiden Motoren getauscht werden. Hierfür ist folgende Grafik recht hilfreich.
Bitte niemals im Eingeschalteten Zustand eine Motorleitung vom Motor oder Mainboard entfernen! 

<img src="https://docs.vorondesign.com/build/startup/images/V2-motor-configuration-guide.png" alt="v2Motorenmove" width=700 height=400>


## Ausrichtung vom Druckbett 
Bevor der 0,0-Punkt und die Position des Z-Endanschlags eingestellt werden, müssen die physischen Positionen des Z-Endanschlags und des Druckbetts endgültig festgelegt werden.

Der Z-Endanschlag sollte sich in einer Linie mit der Düse befinden, wenn sich der Werkzeugkopf in der maximalen Y-Position befindet. Stellt X und Y mit G28 X Y ein und verfahrt dann nur X, um eine Position des Z-Endanschlags bei maximalem Y-Verfahrweg zu finden, bei der der Endanschlag noch ausgelöst wird. Schraubt anschliend den Z-Endanschlag in dieser Position fest

Sobald der Z-Endanschlag in seiner Position fixiert ist, sollte die Grundplatte so eingestellt werden, so dass der Z-Endanschlagstift etwa 2-3 mm von der Aluminiumgrundplatte entfernt ist. Die Grundplatte sollte auf jeder Seite vermessen werden, um sicherzustellen, dass sie zentriert und eben mit der Vorderkante des Rahmens ist. Wenn dabei die Profile, auf denen das B ettmontiert ist, verschoben werden müssen, überprüft den Z-Anschlag anschließend noch einmal, um sicherzustellen, dass er noch erreichbar ist. Beim Anziehen der Befestigungsschrauben für das Bett empfiehlt es sich, eine Schraube fest, zwei halbfest und die letzte locker anzuziehen (am besten im heißen Zustand).


## 0,0-Punkt bestimmen

Die Referenzpunktposition befindet sich nicht an der typischen Position 0,0, sondern an der maximalen Verfahrposition. Die tatsächlichen Werte variieren je nach Größe des Druckers.

Je nach Bettposition müssen die Parameter möglicherweise angepasst werden, um den 0,0-Punkt neu zu positionieren.

Beginnt mit der erneuten Ausführung der Befhele G28 X Y, um X und Y zu initialisieren. Danach befindet sich die Düse an der maximalen X,Y-Position, die durch position_max unter [stepper_x] und [stepper_y] definiert ist.
Fahrt den Druckkopf  in die vordere linke Ecke des Bettes.
Wenn die linke Ecke des Bettes nicht innerhalb von 3-5 mm erreicht werden kann, muss die Position des Bettes physisch angepasst werden (falls möglich). Verschiebt dafür das Bett auf den Profilen, um die Bettposition innerhalb des Bereichs zu erreichen.
Wichtig! Die Düse muss nach der Justierung immer noch den Z-Endschalter erreichen können.
Wenn der Druckkopf sich vorne links befindets tippe M114 in die Konsole ein, um die aktuelle Position abzurufen.
Hinweis: Aufgrund anderer Toleranzen ist es in der Regel nicht empfehlenswert, die Position 0,0 genau auf die Ecke des Bettes zu setzen. Spezifische Bettgrößen sind immer etwas größer als das definierte Druckvolumen, so dass der Druckvolumenverlust minimal ist.

Wenn der X- und Y-Versatz weniger als 1 mm beträgt und 0,0 über dem Bett liegt, muss nichts geändert werden.

Wenn die X- und Y-Versätze innerhalb von 5 mm liegen oder 0,0 über dem Bett liegt, sollten die position_max-Werte angepasst werden. Liegt der 0,0-Punkt über dem Bett, muss der Abstand vom Nullpunkt nach vorne links (position_max) erhöht werden. Liegt der 0,0-Punkt hinter dem Bett, muss der Abstand verringert werden. Der jeweilige Wert wird durch die Ausgabe des Befehls M114 bestimmt. Aktualisieren Sie position_max und position_endstop für beide [stepper_x] und [stepper_y] wie folgt:

    Für X: New = Current - Get Position X (M114) Ergebnis
    Für Y: Neu = Aktuell - Hole Position Y (M114) Ergebnis

Wenn die Position des Z-Endstopps zuvor definiert wurde, müssen Sie den Prozess erneut durchführen, um die Position des Z-Endstopps festzulegen 

Die Änderunge in der printer.cfg werden erst nach Eingabe des Befehls 
```
FIRMWARE_RESTART
```
übernommen.

## Position vom Z-Endstops 
Home zuerst wieder X und Y.
Fahre den Druckkopf in die Position, so dass sich die Düse genau über den Z-Endstop befindet.
Tippe 
```
M114 
```
in die Konsole und notiere dir die Werte für X und Y
Diese Werte müssen jetzt in der printer.cfg unter der Sektion [safe_z_home] eingetargen werden

Beispiel:
```
[safe_z_home]
home_xy_position:204,350
speed:100
z_hop:10
```

Starte Klipper mit 
```
FIRMWARE_RESTART
```
neu. Jetzt sollte der Drucker vollständig mit dem Befehl 
```
G28
```
homen.
     
## Probe

Wenn sich der Werkzeugkopf in der Mitte des Bettes befindet, vergewissern Sie sich, dass der Messtaster richtig funktioniert.

Wenn er weit vom Bett entfernt ist, sollte 
```
QUERY_PROBE
```
"open" anzeigen. 
Wenn sich ein Metallobjekt in der Nähe des Messtasters befindet, sollte QUERY_PROBE "triggered" anzeigen. Wenn das Signal invertiert ist, fügen Sie ein "!" vor der Pin-Definition hinzu.

Verringern Sie langsam die Z-Höhe und führen Sie 
```
QUERY_PROBE 
```
jedes Mal aus, bis QUERY_PROBE "getriggert" anzeigt - stellen Sie sicher, dass die Düse die Druckoberfläche nicht berührt (und Freiraum hat).

### Wiederholgenauigkeit

fahr den Druckkopf mittig übers Bett
```
G90
G1 X175 Y175 F3000
```
Anschließend tippe folgenden befehl in die Konsole ein
```
PROBE_ACCURACY
```
Das Programm tastet das Bett 10 Mal hintereinander ab und gibt am Ende einen Wert für die Standardabweichung aus. 
Vergewisser dich, dass der gemessene Abstand keine Tendenz aufweist (allmählich ab- oder zunimmt während der 10 Messungen) und dass die Standardabweichung weniger als 0,003 mm beträgt.

## Quad Gantry Level 
Da der V2 4 unabhängige Z-Motoren verwendet, muss das gesamte Gantry-System speziell nivelliert werden. Das Makro, mit dem dieser Prozess aufgerufen wird, heißt QUAD_GANTRY_LEVEL (im Sprachgebrauch manchmal auch als 'QGL' bezeichnet). Es prüft jeden der 4 Punkte dreimal, ermittelt den Durchschnitt der Messwerte und nimmt dann Anpassungen vor, bis das Portal waagerecht ausgerichtet ist.
Wenn der Prozess aufgrund eines Fehlers "außerhalb der Grenzen" fehlschlägt, deaktiviert die Schrittmotoren und bewegt das Portal langsam von Hand, bis es annähernd gerade ist.

## Z-Offset-Einstellung
Lasst den Drucker min 15min bei 245°C Hotend und 100°C Bett vorheizen

Vorbereitung (Befehle nacheinander in die Konsole eingeben)
```
G28
QUAD_GANTRY_LEVEL
G28
BED_MESH_CLEAR
G90
G1 X175 Y175 F3000
```
Anschließend folgenden Befehl ausführen
```phyton
Z_ENDSTOP_CALIBRATE
```

Legt ein Blatt Papier unter den Druckkopf auf bett und fhrt den Druckkopf langsam in Richtung Bett, indem ihr den Befehl TESTZ Z=-1 verwenden, bis die Düse relativ nahe am Bett ist.
Mit TESTZ Z=-0.1 könnt ihr anschließend soweit nach unten fahren, bis die Düse leicht über das Papier kratzt. Wenn Ihr zu weit unten seid, dann könnt ihr mit dem Befehl  TESTZ Z=0.1 wieder höher fahren. Es gehen auch kleinen Schritte wie TESTZ Z=-0.025 oder ähnlich. Wenn alles passt gebt ACCEPT und SAVE_CONFIG nacheinander in die Konsole ein
Wichtig: Klipper geht davon aus, dass dieser Prozess kalt durchgeführt wird. Wenn er im heißen Zustand durchgeführt wird, fahrt, nachdem das Papier entfernt wurde, noch einmal mittels TESTZ Z=-0.1 0,1mm runter.


