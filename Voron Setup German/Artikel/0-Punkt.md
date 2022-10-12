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
