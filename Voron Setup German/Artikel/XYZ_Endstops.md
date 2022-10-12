### Endstops kontrollieren
Vergewisser dich, dass keiner der X-, Y- oder Z-Endstopps betätigt ist. 
Tippe dann
```QUERY_ENDSTOPS ```
in die Konsole ein.
Folgendes solltest du dann angezeigt bekommen:
```x:open y:open z:open```
Wenn ein Endschalter ```"triggered"``` statt ```"open"``` anzeigt, überprüfe bitte ob wirklich keiner betätigt ist, der Anschluss (NC) und die 
Leitungen nicht beschädigt sind (kabelbruch, schlechter/kein Kontakt der Stecker oder ähnlich)
Jetzt kannst du den X-Endschalter händisch betätigen und erneut ```QUERY_ENDSTOPS``` in die Konsole eingeben.

Die anzeige sollte nun lauten:
```x:triggered y:open z:open```
Wiederhole den Test auch für den Y und Z Endschalter.

Wenn die Endschalter falsch herum arbeiten, sprich bei Nichtbetätigung schon "triggered" anzeigen, dann geh in die printer.cfg und füge ein ! vor der Pin ein, bzw. entferne es, wenn es schon vorhanden ist. 

als Beispiel:
```aus endstop_pin: P1.28 wird endstop_pin: !P1.28```
