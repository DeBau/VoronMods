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
