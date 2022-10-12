## XY-Homen

Wichtig! Hand auf den Netzschalter, oder mit der Maus in der Weboberfläche auf den Emergency Stop um bei einem Fehler schnell reagieren und den Drucker ausschalten zu können.

Homen von X mit dem Befehl
```
G28 X
```
Die Gantry sollte etwas hochfahren und anschließend sollte sich der Druckkopf nach rechts bewegen, bis er den X-Endschalter auslöst
Wenn er sich in eine andere Richtung bewegt, brechen Sie den Vorgang ab, notieren Sie sich das und fahren Sie mit dem Y-Test fort

Homen von Y mit dem Befehl
```
G28 Y
```
Der Druckkopf sollte nach hinten bewegen, bis er den Y-Endschalter auslöst.

Bei dem CoreXY-Systme müssen sich beide Motoren bewegen, damit sich der Druckkopf nur in X- oder Y-Richtung bewegt. 
Wenn sich die Gantry zuerst nach unten bewegt, bevor sich der Druckkopf nach rechts bewegt, müssen Sie die Z-Stepper-Richtungen in der Konfiguration umkehren.

Sollten die Axen falsch fahren, muss eventuell der DIR-PIN von den AB Motoren negiert, oder die Anschlüsse der beiden Motoren getauscht werden. 
Hierfür ist folgende Grafik recht hilfreich.
Bitte niemals im Eingeschalteten Zustand eine Motorleitung vom Motor oder Mainboard entfernen! 

<img src="https://docs.vorondesign.com/build/startup/images/V2-motor-configuration-guide.png" alt="v2Motorenmove" width=700 height=400>

