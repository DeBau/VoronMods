## Initial Setup für einen Voron V2 350
- Octopus v1.1 Board
- TMC2209 Treiber
- Moons Motoren (17HS19-2004S und 17HS08-1004S)
- Omron TL-Q5MC2-Z Sensor
- mini12864 LC Display

Optional CANBus

- BTT EBB36 v1.2
- BTT U2C V2


### Quellen: 
- [Voron Design Dokumentation](https://docs.vorondesign.com)
- [Voron Design Github Voron v2](https://docs.vorondesign.com)
- [Klipper](https://www.klipper3d.org/)

Viele Stellen wurden entsprechend aus den Quellen ins Deutsche übersetzt 

#### Support
Wenn ihr mir einen Kaffee ausgeben wollt: 
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/dfoure?country.x=DE&locale.x=de_DE)


## Vorbereitungen
### Download der Voron printer.cfg
- [Voron Design printer.cfg für das Octopus Board](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations/Octopus)
### PinOut vom Octopus Board
<img src="https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/BIGTREETECH-Octopus-1.1-color-PIN.jpg" alt="pinout" width=800 height=400>

# Inhaltsverzeichnis
## Grundeintellungen
*Notwendige Anpassungen*
- [printer.cfg](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/printer.md) 
##### Infos rund um die printer.cfg und deren Einstellungen 
- [Motorstromberechnung](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Motorstrom.md)
- [TMC Treiber](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/TMC_Treiber.md)
## Temperaturen
- [Thermistoren und Heater](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Temperaturen.md) 
- [PID Tuning](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/PID_Tuning.md) 
## Motion
*:warning: Gehe hier der Reihe nach vor. 1-8*
1. [Stepper überprüfen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Stepper.md) 
2. [XYZ](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/XYZ_Endstops.md) 
3. [XY Homen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/XY_Homen.md) 
4. [Druckbettausrichtung](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Ausrichtung.md) 
5. [0-Punkt Bestimmung](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/0-Punkt.md) 
6. [Z-Endstop Position festlegen](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/Z_Endstop.md) 
7. [Probe Checks](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/probe.md) 
8. [QGL](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/qgl.md) 
## Offsets
[Z-Offset ermitteln](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/z-offset.md) 

## CANBus
[CANBus](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/Artikel/CanBus.md) 





     


