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

Wer sich mit den Force Move auskennt, wird wissen was er noch in die printer.cfg einfügen muss um die Befehle zum laufen zu bekommen ;)

### Endstops kontrollieren

Vergewisser dich, dass keiner der X-, Y- oder Z-Endstopps gedrückt wird. 
Tippe dann QUERY_ENDSTOPS in die Konsole ein.
Folgendes sollte du dann angezeigt bekommen:

- Senden: QUERY_ENDSTOPS
- Empfangen: x:offen y:offen z:offen

Wenn ein Endschalter "triggered" statt "open" anzeigt, überprüfe bitte ob wirklich keiner betätigt ist. 
Jetzt kannst du den X-Endschalter händisch betätigen und erneut QUERY_ENDSTOPS in die Konsole eingeben.

Die anzeige sollte nun lautet:
- x:triggerd y:offen z:offen

Wiederhole den Test auch für den Y und Z Endschalter

Wenn die Endschalter falsch herum arbeiten, sprich bei Nichtbetätigung schon "triggerd" anzeigen, dann geh in die printer.cfg und füge ein ! vor der Pin ein, bzw. entferne es, wenn es schon vorhanden ist. 

als Beispiel:

- aus endstop_pin: P1.28 wird endstop_pin: !P1.28


## XY-Homen

Wichtig: Hand auf den Netzschalter, oder mit der Maus in der Weboberfläche auf den Emergency Stop um bei einem Fehler schnell reagieren und den Drucker ausschalten zu können.

Homen von X mit dem Befehl

- G28 X

Homen von Y mit dem Befehl

- G28 Y

Sollten die Axen falsch fahren, muss evntuell der DIR-PIN von den AB Motoren negiert, oder die Anschlüsse der beiden Motoren getauscht werden. Hoerfür ist folgende Grafik recht hilfreich.
Bitte niemals im Eingeschalteten Zustand eine Motorleitung vom Motor oder Mainboard entfernen! 

<img src="https://docs.vorondesign.com/build/startup/images/V2-motor-configuration-guide.png" alt="v2Motorenmove" width=700 height=400>



    
    
## Bettpositionierung (V2)

Die Positionierung des Druckbetts ist bei der V2 viel einstellbarer als bei den anderen Modellen. Bevor der 0,0-Punkt und die Position des Z-Endanschlags eingestellt werden, müssen die physischen Positionen des Z-Endanschlags und des Druckbetts endgültig festgelegt werden.

Der Z-Endanschlag sollte sich in einer Linie mit der Düse befinden, wenn sich der Werkzeugkopf in der maximalen Y-Position befindet. Stellt X und Y mit G28 X Y ein und verfahrt dann nur X, um eine Position des Z-Endanschlags bei maximalem Y-Verfahrweg zu finden, bei der der Endanschlag noch ausgelöst wird. Schraubt anschliend den Z-Endanschlag in dieser Position fest

Sobald der Z-Endanschlag in seiner Position fixiert ist, sollte die Grundplatte so eingestellt werden, dass der Z-Endanschlagstift etwa 2-3 mm von der Aluminiumgrundplatte entfernt ist. Die Grundplatte sollte auf jeder Seite vermessen werden, um sicherzustellen, dass sie zentriert und eben mit der Vorderkante des Rahmens ist. Wenn dabei die Profile, auf denen der Sockel montiert ist, verschoben werden müssen, überprüft den Z-Anschlag noch einmal, um sicherzustellen, dass er noch erreichbar ist. Beim Anziehen der Befestigungsschrauben für das Bett empfiehlt es sich, eine Schraube fest anzuziehen, zwei halbfest und die letzte locker anzuziehen (am besten im heißen Zustand).


## 0,0-Punkt bestimmen

Die Referenzpunktposition befindet sich nicht an der typischen Position 0,0, sondern an der maximalen Verfahrposition. Die tatsächlichen Werte variieren je nach Größe des Druckers.

Je nach Bettposition müssen die Positionsparameter möglicherweise angepasst werden, um den 0,0-Punkt neu zu positionieren.

Beginnt mit der erneuten Ausführung der Befhele G28 X Y, um X und Y zu initialisieren. Danach befindet sich die Düse an der maximalen X,Y-Position, die durch position_max unter [stepper_x] und [stepper_y] definiert ist.
Fahrt den Druckkopf  in die vordere linke Ecke des Bettes.
Wenn die linke Ecke des Bettes nicht innerhalb von 3-5 mm erreicht werden kann, muss die Position des Bettes physisch angepasst werden (falls möglich). Verschiebt dafür das Bett auf den Profilen, um die Bettposition innerhalb des Bereichs zu erreichen.
Wichtig! Diese Düse muss nach der Justierung immer noch den Z-Endschalter erreichen können.
Sobald sich die Düse in der Nähe der vorderen linken Ecke des Bettes befindet, aber noch auf dem Bett, senden Sie einen M114-Befehl, um die aktuelle Position abzurufen.
Hinweis: Aufgrund anderer Toleranzen ist es in der Regel nicht empfehlenswert, die Position 0,0 genau auf die Ecke des Bettes zu setzen. Spezifische Bettgrößen sind immer etwas größer als das definierte Druckvolumen, so dass der Druckvolumenverlust minimal ist.

Wenn der X- und Y-Versatz weniger als 1 mm beträgt und 0,0 über dem Bett liegt, muss nichts geändert werden.

Wenn die X- und Y-Versätze innerhalb von 5 mm liegen oder 0,0 über dem Bett liegt, sollten die position_max-Werte angepasst werden, um zu ändern, wo der 0,0-Punkt berechnet wird. Liegt der 0,0-Punkt über dem Bett, muss der Abstand vom Nullpunkt nach vorne links (position_max) erhöht werden. Liegt der 0,0-Punkt hinter dem Bett, muss der Abstand verringert werden. Der Betrag wird durch die Ausgabe des Befehls M114 bestimmt. Aktualisieren Sie position_max und position_endstop für beide [stepper_x] und [stepper_y] wie folgt:

    Für X: New = Current - Get Position X (M114) Ergebnis
    Für Y: Neu = Aktuell - Hole Position Y (M114) Ergebnis

Wenn die Position des Z-Endstopps zuvor definiert wurde, müssen Sie den Prozess erneut durchführen, um die Position des Z-Endstopps festzulegen (falls zutreffend).

Wenn etwas in der Druckerkonfigurationsdatei aktualisiert wurde, speichern Sie die Datei und starten Sie Klipper mit FIRMWARE_RESTART neu.


## Lage der Z-Endanschlagstifte (V1, Trident, V2, Legacy)

Führen Sie zunächst G28 X Y erneut aus, um X und Y zu fixieren.
Bewegen Sie die Düse mithilfe der Softwaresteuerung, bis sie sich direkt über dem Z-Endanschlagschalter befindet.
Senden Sie einen M114-Befehl und zeichnen Sie die X- und Y-Werte auf.
Aktualisieren Sie die Referenzfahrtroute in der Druckerkonfigurationsdatei unter [homing_override] oder [safe_z_home] mit diesen Werten.
Starten Sie Klipper mit FIRMWARE_RESTART neu.
Führen Sie einen vollständigen G28 aus und vergewissern Sie sich, dass der Drucker die X-, Y- und Z-Werte korrekt ausrichtet.
    
    
## Testen des Messtasters

Wenn sich der Werkzeugkopf in der Mitte des Bettes befindet, vergewissern Sie sich, dass der Messtaster richtig funktioniert.

Wenn er weit vom Bett entfernt ist, sollte QUERY_PROBE "offen" zurückgeben. Wenn sich ein Metallobjekt in der Nähe des Messtasters befindet, sollte QUERY_PROBE "ausgelöst" zurückgeben. Wenn das Signal invertiert ist, fügen Sie ein "!" vor der Pin-Definition hinzu (z. B. pin: ! z:P1.24).

Verringern Sie langsam die Z-Höhe und führen Sie QUERY_PROBE jedes Mal aus, bis QUERY_PROBE "getriggert" zurückgibt - stellen Sie sicher, dass die Düse die Druckoberfläche nicht berührt (und Freiraum hat).
Überprüfung der Tastkopfgenauigkeit

Bewegen Sie die Sonde bei (vorerst) kaltem Bett und Hotend in die Mitte des Bettes und führen Sie PROBE_ACCURACY aus. Das Programm tastet das Bett 10 Mal hintereinander ab und gibt am Ende einen Wert für die Standardabweichung aus. Vergewissern Sie sich, dass der gemessene Abstand keine Tendenz aufweist (allmählich ab- oder zunimmt während der 10 Messungen) und dass die Standardabweichung weniger als 0,003 mm beträgt.

Beispiel für eine instabile PROBE_ACCURACY (Tendenz nach unten während der Aufwärmphase).

## Quad Gantry Level (V2)

Da die V2 4 unabhängige Z-Motoren verwendet, muss das gesamte Gantry-System speziell nivelliert werden. Das Makro, mit dem dieser Prozess aufgerufen wird, heißt QUAD_GANTRY_LEVEL (im Sprachgebrauch manchmal auch als 'QGL' bezeichnet). Es prüft jeden der 4 Punkte dreimal, ermittelt den Durchschnitt der Messwerte und nimmt dann Anpassungen vor, bis das Portal waagerecht ausgerichtet ist.

Wenn der Prozess aufgrund eines Fehlers "außerhalb der Grenzen" fehlschlägt, deaktivieren Sie die Schrittmotoren und bewegen Sie das Portal oder Bett langsam von Hand, bis es annähernd eben ist. Richten Sie Ihren Drucker (G28) wieder ein und führen Sie die Sequenz erneut aus. Möglicherweise müssen Sie die Sequenz mehr als einmal ausführen. Vergewissern Sie sich, dass der Einstellwert für jeden Schrittmotor auf 0 konvergiert. Wenn er abweicht, überprüfen Sie, ob die Schrittmotoren mit dem richtigen Schrittmotor-Treiber verdrahtet sind (siehe Dokumentation).


## Z-Offset-Einstellung

Wenn Sie keine PID-Einstellung vorgenommen haben, stellen Sie den Extruder auf 245 °C und das Heizbett auf 100 °C ein und lassen Sie den Drucker mindestens 15 Minuten lang aufheizen.
Anfänglicher / einfacher Prozess

Vorbereitung

    V1/Trident: Führen Sie einen G28, dann einen Z_TILT_ADJUST und dann einen weiteren G28 aus.
    V2: Führen Sie einen G28 aus, dann einen QUAD_GANTRY_LEVEL und dann einen weiteren G28.
    Alle anderen: Führen Sie einen G28 aus.
    Bewegen Sie die Düse in die Mitte des Bettes, wenn sie sich nicht bereits dort befindet.
    Löschen Sie alle gespeicherten Bettnetze mit BED_MESH_CLEAR

Führen Sie Z_ENDSTOP_CALIBRATE (V0, Trident, V2) oder PROBE_CALIBRATE (Switchwire) aus.

Bewegen Sie die Düse langsam in Richtung Bett, indem Sie TESTZ Z=-1 verwenden, bis die Düse relativ nahe am Bett ist, und gehen Sie dann mit TESTZ Z=-0,1 nach unten, bis die Düse ein Stück Papier auf der Bauplatte berührt. Wenn Sie zu weit nach unten gehen, können Sie die Düse wieder nach oben bewegen mit: TESTZ Z=0.1. Wenn Sie mit der Düsenhöhe zufrieden sind, führen Sie ACCEPT und dann SAVE_CONFIG aus.

Wichtig: Klipper geht davon aus, dass dieser Prozess kalt durchgeführt wird. Wenn er im heißen Zustand durchgeführt wird, führen Sie einen zusätzlichen TESTZ Z=-0.1 durch, bevor Sie ihn akzeptieren.

Wenn ein "out of bounds"-Fehler auftritt, senden Sie Z_ENDSTOP_CALIBRATE, ACCEPT und dann SAVE_CONFIG. Dadurch wird die 0-Bett-Höhe neu definiert, so dass Sie näher herankommen können.

V2: Wenn Sie diese Fehlermeldung erhalten, bedeutet dies wahrscheinlich, dass die Welle für den Z-Endstopp zu lang ist und während des Drucks am Druckkopf hängen bleiben kann. Am besten ist es, die Welle zu kürzen oder das Bett (z. B. mit einer Unterlegscheibe) so anzuheben, dass es weniger als 1 mm von der Bauoberfläche entfernt ist.
