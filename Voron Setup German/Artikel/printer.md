Ersteinmal kümmern wir uns um die Printer.cfg und kommentieren die entsprechenden Bereiche für unseren Drucker aus.
Alles was nicht benötigt wird, kann und sollte wegen der Übersichtlichkeit gelöscht werden.
Für einen Voron V2 350er sieht die fertige printer.cfg dann so aus:

- [Bespiel printer.cfg](https://github.com/DeBau/VoronMods/blob/main/Voron%20Setup%20German/printer.cfg)

Natürlich ist diese printer.cfg nicht allgemein gültig! 
Sie kann sich je nach verwendeten Motoren, Sensoren, Extruder, Board, Z-Endstop Position, Probe etc unterscheiden und sollte hier
nur der Veranschaulichung dienen! Bitte nicht 1:1 übernehmen

## Fangen wir von vorne an 

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
  
#### MCU ID
diese muss ermittelt werden, nähere Infos findet ihr hier [MCU ID beim Octopus Board](https://docs.vorondesign.com/build/software/octopus_klipper.html)

Original Abschnitt der printer.cfg
```
##--------------------------------------------------------------------
serial: /dev/serial/by-id/{REPLACE WITH YOUR SERIAL}
restart_method: command
##--------------------------------------------------------------------
```
wir setzen die ermittelte ID ein und erhalten:
```
[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f446xx_1B0046000650534E4E313120-if00
restart_method: command
```
#### Stepper X
Original:
```
#####################################################################
#   X/Y Stepper Settings
#####################################################################

##  B Stepper - Left
##  Connected to MOTOR_0
##  Endstop connected to DIAG_0
[stepper_x]
step_pin: PF13
dir_pin: !PF12
enable_pin: !PF14
rotation_distance: 40
microsteps: 32
full_steps_per_rotation:200  #set to 400 for 0.9 degree stepper
endstop_pin: PG6
position_min: 0
##--------------------------------------------------------------------

##  Uncomment below for 250mm build
#position_endstop: 250
#position_max: 250

##  Uncomment for 300mm build
#position_endstop: 300
#position_max: 300

##  Uncomment for 350mm build
#position_endstop: 350
#position_max: 350

##--------------------------------------------------------------------
homing_speed: 25  #Max 100
homing_retract_dist: 5
homing_positive_dir: true
```
angepasst auf den 350er v2
```
#####################################################################
#   X/Y Stepper Settings
#####################################################################

##  B Stepper - Left
##  Connected to MOTOR_0
##  Endstop connected to DIAG_0
[stepper_x]
step_pin: PF13
dir_pin: !PF12
enable_pin: !PF14
rotation_distance: 40
microsteps: 32
endstop_pin: PG6
position_min: 0
position_endstop: 350
position_max: 350
homing_speed: 50  
homing_retract_dist: 5
homing_positive_dir: true
```
#### Stepper Y
Original:
```
[#  A Stepper - Right
##  Connected to MOTOR_1
##  Endstop connected to DIAG_1
[stepper_y]
step_pin: PG0
dir_pin: !PG1
enable_pin: !PF15
rotation_distance: 40
microsteps: 32
full_steps_per_rotation:200  #set to 400 for 0.9 degree stepper
endstop_pin: PG9
position_min: 0
##--------------------------------------------------------------------

##  Uncomment for 250mm build
#position_endstop: 250
#position_max: 250

##  Uncomment for 300mm build
#position_endstop: 300
#position_max: 300

##  Uncomment for 350mm build
#position_endstop: 350
#position_max: 350

##--------------------------------------------------------------------
homing_speed: 25  #Max 100
homing_retract_dist: 5
homing_positive_dir: true
```
angepasst auf den v2 350er
```
[stepper_y]
step_pin: PG0
dir_pin: !PG1
enable_pin: !PF15
rotation_distance: 40
microsteps: 32
endstop_pin: PG9
position_min: 0
position_endstop: 350
position_max: 350
homing_speed: 50  
homing_retract_dist: 5
homing_positive_dir: true
```
#### Stepper Z
Original
```
## Z0 Stepper - Front Left
##  Connected to MOTOR_2
##  Endstop connected to DIAG_2
[stepper_z]
step_pin: PF11
dir_pin: !PG3
enable_pin: !PG5
rotation_distance: 40
gear_ratio: 80:16
microsteps: 32
endstop_pin: PG10
##  Z-position of nozzle (in mm) to z-endstop trigger point relative to print surface (Z0)
##  (+) value = endstop above Z0, (-) value = endstop below
##  Increasing position_endstop brings nozzle closer to the bed
##  After you run Z_ENDSTOP_CALIBRATE, position_endstop will be stored at the very end of your config
position_endstop: -0.5
##--------------------------------------------------------------------

##  Uncomment below for 250mm build
#position_max: 210

##  Uncomment below for 300mm build
#position_max: 260

##  Uncomment below for 350mm build
#position_max: 310

##--------------------------------------------------------------------
position_min: -5
homing_speed: 8
second_homing_speed: 3
homing_retract_dist: 3
```
angepasst auf den V2 350er
```
## Z0 Stepper - Front Left
##  Connected to MOTOR_2
##  Endstop connected to DIAG_2
[stepper_z]
step_pin: PF11
dir_pin: !PG3
enable_pin: !PG5
rotation_distance: 40
gear_ratio: 80:16
microsteps: 32
endstop_pin: PG10
position_endstop: +0.5
position_max: 310
position_min: -5
homing_speed: 8
second_homing_speed: 3
homing_retract_dist: 3
```
Hierbei gint es eine Besonderheit: Euren Z-Endstop. Wenn dieser über das Bett hinausragt, tragt bei
```position_endstop``` einen positiven Wert ein, wenn er unterhalb des Bettes steht, dann einen negativen. Der korrekte Wert wird später mittels
```Z_ENDSTOP_CALIBRATE```ermittelt.

#### Extruder
Original:
```
[extruder]
step_pin: PE2
dir_pin: PE3
enable_pin: !PD4
##  Update value below when you perform extruder calibration
##  If you ask for 100mm of filament, but in reality it is 98mm:
##  rotation_distance = <previous_rotation_distance> * <actual_extrude_distance> / 100
##  22.6789511 is a good starting point
rotation_distance: 22.6789511   #Bondtech 5mm Drive Gears
##  Update Gear Ratio depending on your Extruder Type
##  Use 50:17 for Afterburner/Clockwork (BMG Gear Ratio)
##  Use 80:20 for M4, M3.1
gear_ratio: 50:17               #BMG Gear Ratio
microsteps: 32
full_steps_per_rotation: 200    #200 for 1.8 degree, 400 for 0.9 degree
nozzle_diameter: 0.400
filament_diameter: 1.75
heater_pin: PA2
## Check what thermistor type you have. See https://www.klipper3d.org/Config_Reference.html#common-thermistors for common thermistor types.
## Use "Generic 3950" for NTC 100k 3950 thermistors
#sensor_type:
sensor_pin: PF4
min_temp: 10
max_temp: 270
max_power: 1.0
min_extrude_temp: 170
control = pid
pid_kp = 26.213
pid_ki = 1.304
pid_kd = 131.721
##  Try to keep pressure_advance below 1.0
#pressure_advance: 0.05
##  Default is 0.040, leave stock
#pressure_advance_smooth_time: 0.040
```
angepasst auf den CW1 mit einem Thermistor vom ```Type ATC Semitec 104GT-2```
```
##  Connected to MOTOR_6
##  Heater - HE0
##  Thermistor - T0
[extruder]
step_pin: PE2
dir_pin: PE3
enable_pin: !PD4
rotation_distance: 22.6789511  
gear_ratio: 50:17              
microsteps: 32
full_steps_per_rotation: 200    
nozzle_diameter: 0.400
filament_diameter: 1.75
heater_pin: PA2
sensor_type: ATC Semitec 104GT-2
sensor_pin: PF4
min_temp: 10
max_temp: 270
max_power: 1.0
min_extrude_temp: 170
control = pid
pid_kp = 26.213
pid_ki = 1.304
pid_kd = 131.721
#pressure_advance: 0.05
#pressure_advance_smooth_time: 0.040
```
### Sensortypen

- EPCOS 100K B57560G104F
- ATC Semitec 104GT-2
- ATC Semitec 104NT-4-R025H42G
- Generic 3950
- Honeywell 100K 135-104LAG-J01
- NTC 100K MGB18-104F39050L32
- SliceEngineering 450
- TDK NTCG104LH104JT1



### Berechnung von Motorstrom

Bei Verwendung der Treibertypen 2208 / 2209 werden Spannung und Strom in der Software eingestellt. 
In Klipper haben die Motorströme zwei Einstellungen: 

- run_current 
- hold_current

Für die meisten Motoren ist es jedoch nicht mehr empfehlenswert, einen Haltestrom anzugeben. Die Stromeinstellungen in Klipper basieren auf dem Effektivwert (RMS) und nicht auf dem Spitzenstrom. In den technischen Datenblättern der meisten Motoren ist die Spitzenstromkapazität angegeben.

#### Berechnung der Ströme

Um die maximalen Strom für einen Stepper zu berechnen, geh wie folgt vor:
- Schau ins Datenblatt vom Schrittmotor
- Multipliziere den Spitzenstrom mit 0,707, um den maximalen Strom  (RMS) zu ermitteln.

##### Beispiel

Der LDO 42STH130-1684 ist mit einem maximalen Strom von 1,68 Ampere spezifiziert. Der maximale Laufstrom beträgt 1,68 * .707 = 1,1877, abgerundet auf einen maximalen RMS von 1,1 Ampere. 

Unabhängig vom berechneten Maximalstrom des Motors beträgt der maximale Strom vom 2209-Treibers 1,2 Ampere. Die Berechnung bezieht sich also auf den Höchstwert, den der Motor verarbeiten kann. Es wird empfohlen, mit kleineren Werten zu beginnen und von dort aus zu arbeiten. Wenn Sie mit einem hohen Wert beginnen, kann dies zu geschmolzenen Motorhalterungen führen.

#### Welchen Modus für meine Treiber?

Standardmäßig setzt Klipper die TMC-Treiber in den "spreadCycle"-Modus. 
Wenn der Treiber "stealthChop" unterstützt, kann er durch Hinzufügen von stealthchop_threshold: 999999 in die TMC-Sektion in diesem Modus betrieben werden.

Im Allgemeinen bietet der SpreadCycle-Modus ein größeres Drehmoment und eine höhere Positionsgenauigkeit als der StealthChop-Modus. Allerdings kann der stealthChop-Modus bei einigen Druckern deutlich weniger hörbare Geräusche erzeugen.

Tests, bei denen die Modi verglichen wurden, haben gezeigt, dass sich die "Positionsverzögerung" bei Bewegungen mit konstanter Geschwindigkeit im stealthChop-Modus um etwa 75 % eines Vollschritts erhöht (bei einem Drucker mit 40 mm Rotationsabstand und 200 Schritten pro Umdrehung erhöhte sich die Positionsabweichung bei Bewegungen mit konstanter Geschwindigkeit beispielsweise um ~0,150 mm). Diese "Verzögerung beim Erreichen der angeforderten Position" kann sich jedoch nicht als signifikanter Druckfehler erweisen und man kann das ruhigere Verhalten des stealthChop-Modus vorziehen.

Es wird empfohlen, immer den Modus "spreadCycle" (ohne Angabe von stealthchop_threshold) oder immer den Modus "stealthChop" (mit stealthchop_threshold auf 999999) zu verwenden. Leider liefern die Treiber oft schlechte und verwirrende Ergebnisse, wenn der Modus geändert wird, während der Motor eine Geschwindigkeit ungleich Null hat.



