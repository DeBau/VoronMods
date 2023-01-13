```
sudo apt-get install git -y
git clone https://github.com/Arksine/CanBoot
```

```
cd CanBoot
make menuconfig
```

```
cd ~/klipper
make clean
make menuconfig
```

```
cd ~/CanBoot/scripts
pip3 install pyserial

python3 flash_can.py -f ~/klipper/octopus_klipper.bin -d <serial_device>
```

```
cd
sudo nano /etc/network/interfaces.d/can0
```

```
allow-hotplug can0
iface can0 can static
    bitrate 500000
    up ifconfig $IFACE txqueuelen 128
```

```
[mcu EBB]
canbus_uuid: 330a31adf6de
```

```
[adxl345]
cs_pin: EBB:PB12
spi_software_sclk_pin: EBB:PB10
spi_software_mosi_pin: EBB:PB11
spi_software_miso_pin: EBB:PB2
axes_map: x,y,z

[resonance_tester]
accel_chip: adxl345
probe_points:
    150,150,20 
```

```
[extruder]
step_pin: EBB:PD0
dir_pin: !EBB:PD1
enable_pin: !EBB:PD2
heater_pin: EBB:PA2
sensor_pin: EBB:PA3
```
```
[tmc2209 extruder]
uart_pin: EBB:PA15
```
```
[probe]
pin: EBB:PB8 
```
```
[fan]
pin: EBB:PA0
```
```
[heater_fan hotend_fan]
pin: EBB:PA1 
```
```
[temperature_sensor ebb_temp]
sensor_type: temperature_mcu
sensor_mcu: EBB
min_temp: 0
max_temp: 120
```

