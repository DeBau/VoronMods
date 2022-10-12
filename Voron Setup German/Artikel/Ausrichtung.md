### Ausrichtung vom Druckbett 
Bevor der 0,0-Punkt und die Position des Z-Endanschlags eingestellt werden, müssen die physischen Positionen des Z-Endanschlags und des Druckbetts endgültig festgelegt werden.

Der Z-Endschalter sollte sich in einer Linie mit der Düse befinden, wenn sich der Druckkopf in der maximalen Y-Position befindet. 
Zuerst homen wir wieder die Achsen X und Y mit folgenden Befehlen
```
G28 X
G28 Y
```
Verfahrt dann nur die X-Achse, um eine Position des Z-Endanschlags bei maximalem Y-Verfahrweg zu finden, bei der der Endschalter noch ausgelöst werden kann. Schraubt anschliend den Z-Endanschlag in dieser Position fest

Sobald der Z-Endanschlag in seiner Position fixiert ist, sollte das Druckbett so eingestellt werden, dass der Z-Endschalter ca. 2-3 mm vom Druckbett entfernt ist. 
Das Druckbett sollte auf jeder Seite vermessen werden, um sicherzustellen, dass es zentriert und eben mit der Vorderkante des Rahmens ist. 
Wenn dabei die Profile, auf denen das Bett montiert ist, verschoben werden müssen, überprüft den Z-Endschalter anschließend noch einmal, um sicherzustellen, 
dass er noch erreichbar ist. 
Beim Anziehen der Befestigungsschrauben für das Bett empfiehlt es sich, eine Schraube fest, zwei halbfest und die letzte locker anzuziehen 
(am besten im heißen Zustand).
