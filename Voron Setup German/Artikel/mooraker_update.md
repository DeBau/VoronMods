
## Keine Config Dateien mehr nach dem Moonraker Update?

Keine Panik, das bekommen wir sehr leicht wieder hin ;)

Loggt euch mit Putty auf eurem Raspberry Pi ein und gebt folgende Befehle der Reihe nach ein

```
cd ~/moonraker
git pull
./scripts/data-path-fix.sh
```
