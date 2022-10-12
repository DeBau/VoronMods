## Diese Anleitung basiert auf einem Voron V2 350er mit Octopus Board ohne Mods

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

### Überprüfen der Temepraturen für das Hotend und Heizbett


<img src="https://docs.vorondesign.com/build/startup/images/mainsail_temp_graph.png" alt="Temperaturen" width=400 height=400>

