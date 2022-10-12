## Diese Anleitung basiert auf einem Voron V2 350er mit Octopus Board ohne Mods

### Quellen: 
- [Voron Design Dokumentation](https://docs.vorondesign.com)
- [Voron Design Github Voron v2](https://docs.vorondesign.com)

## Vorbereitungen
### Download der Voron printer.cfg
- [Voron Design prnter.cfg für das Octopus Board](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus)
- 

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

### Überprüfen der Temperaturen für das Hotend und Heizbett

Die angezeigten Temperaturen sollten im Ausgangszustand ungefähr die aktuelle Raumtemperatur anzeigen.
Ist das nicht der Fall, sollte der Seonsortyp und der Sensorpin überprüft werden
- [Sensortypen]([https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus](https://www.klipper3d.org/Config_Reference.html?h=common+thermistors+thermistor#common-thermistors)


<img src="https://docs.vorondesign.com/build/startup/images/mainsail_temp_graph.png" alt="Temperaturen" width=600 height=400>

