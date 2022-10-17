
## Keine Config Dateien mehr nach dem Moonraker Update?

Keine Panik, das bekommen wir sehr leicht wieder hin ;)

Loggt euch mit Putty auf eurem Raspberry Pi ein und gebt folgende Befehle der Reihe nach ein

```
cd ~/moonraker
git pull
./scripts/data-path-fix.sh
sudo service moonraker restart
```



#### Drucken nicht möglich, Pfade nicht vorhanden etc.....

```
ls -l ~/printer_data
```
https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/printer_data_symlink.png

Delete & symlink
----------------
```
sudo service moonraker stop
cd ~/printer_data
rm -rf gcodes
ln -s ~/gcode_files gcodes
rm -rf database
ln -s ~/.moonraker_database database
rm -rf logs
ln -s ~/klipper_logs ~/printer_data/logs
sudo service moonraker start
```

