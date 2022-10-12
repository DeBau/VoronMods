Die Referenzpunktposition befindet sich nicht an der typischen Position 0,0, sondern an der maximalen Verfahrposition. 
Die tatsächlichen Werte variieren je nach Größe des Druckers.

Je nach Bettposition müssen die Parameter möglicherweise angepasst werden, um den 0,0-Punkt neu zu positionieren.

Wir fangen wieder mit dem "homen" der X- und Y-Achsen an
```
G28 X
G28 Y
```
Der Druckkopf befindet sich jetzt an der maximalen X-,Y-Position hinten rechts
Die Positionen werden in der ```printer.cfg``` durch die```position_max```der Sektionen ```[stepper_x]``` und ```[stepper_y]``` definiert.
Fahrt den Druckkopf  in die vordere linke Ecke des Bettes. Position 0,0
```
G90
G1 X0 Y0 F3000
```

Wenn die linke Ecke des Bettes nicht innerhalb von 3-5 mm erreicht werden kann, muss die Position des Bettes physisch angepasst werden (falls möglich). Verschiebt dafür das Bett auf den Profilen, um die Bettposition innerhalb des Bereichs zu erreichen.

:warning: Wichtig! Die Nozzle muss nach der Justierung immer noch den Z-Endschalter erreichen können.
Wenn der Druckkopf sich vorne links befindets tippe ```M114``` in die Konsole ein, um die aktuelle Position abzurufen.
Hinweis: Aufgrund anderer Toleranzen ist es in der Regel nicht empfehlenswert, die Position 0,0 genau auf die Ecke des Bettes zu setzen. Spezifische Bettgrößen sind immer etwas größer als das definierte Druckvolumen, so dass der Druckvolumenverlust minimal ist.

Wenn der X- und Y-Versatz weniger als 1 mm beträgt und 0,0 über dem Bett liegt, muss nichts geändert werden.

Wenn die X- und Y-Versätze innerhalb von 5 mm liegen oder 0,0 über dem Bett liegt, sollten die ```position_max-Werte``` angepasst werden. 

Liegt der 0,0-Punkt über dem Bett, muss der Abstand vom Nullpunkt nach vorne links ```position_max``` erhöht werden. 

Liegt der 0,0-Punkt hinter dem Bett, muss der Abstand verringert werden. 

Der jeweilige Wert wird durch die Ausgabe des Befehls ```M114``` bestimmt. 

Aktualisieren Sie ```position_max``` und ```position_endstop``` für beide [stepper_x] und [stepper_y] wie folgt:
```
    Für X: Neu = Aktuell  - (M114) X Ergebnis
    Für Y: Neu = Aktuell  - (M114) Y Ergebnis
```
Wenn die Position des Z-Endstopps zuvor definiert wurde, müssen Sie den Prozess erneut durchführen, um die Position des Z-Endstopps festzulegen 

Die Änderunge in der printer.cfg werden erst nach Eingabe des Befehls 
```
FIRMWARE_RESTART
```
übernommen.
