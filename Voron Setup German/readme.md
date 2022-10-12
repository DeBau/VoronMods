## Diese Anleitung basiert auf einem Voron V2 350er mit Octopus Board ohne Mods

### Quellen: 
- [Voron Design Dokumentation](https://docs.vorondesign.com)
- [Voron Design Github Voron v2](https://docs.vorondesign.com)

## Vorbereitungen
### Download der Voron printer.cfg
- [Voron Design prnter.cfg für das Octopus Board](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus)

### Einstellungen in der printer.cfg die unbedingt überprüft werden müssen

## *** THINGS TO CHANGE/CHECK: ***
## MCU paths                            [mcu] section
## Thermistor types                     [extruder] and [heater_bed] sections
## Z Endstop Switch location            [safe_z_home] section
## Homing end position                  [gcode_macro G32] section
## Z Endstop Switch  offset for Z0      [stepper_z] section
## Probe points                         [quad_gantry_level] section
## Min & Max gantry corner postions     [quad_gantry_level] section
## PID tune                             [extruder] and [heater_bed] sections
## Probe pin                            [probe] section
## Fine tune E steps                    [extruder] section
