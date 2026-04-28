install  mainsailOs by raspberry pi imager

### for  Rpi3 please install 32 bit lite OS and then Kiauh and rest of klipper stuff


```shell
sudo apt-get update && sudo apt update && sudo apt upgrade -y
```

if git not available then 
```shell
sudo apt-get install git -y
```

install tft35 
```shell
https://github.com/katzenjens/lcd32
```
or 
```shell
https://github.com/Bobbik1/P3steel_files_and-_scripts/blob/main/tft35
```
```shell
cd ~ && git clone https://github.com/dw-0/kiauh.git
./kiauh/kiauh.sh
```

install klipper screen from kiauh

```shell
sudo reboot
```

voila!

## you may lower down SPI a bit
In your 
```shell
/boot/firmware/config.txt
```
 change:
 ```shell
dtoverlay=waveshare35a:rotate=90
```
to:
```shell
dtoverlay=waveshare35a:rotate=90,speed=16000000
```
### and turn off hdmi
```shell
# hdmi_force_hotplug=1
# hdmi_group=2
# hdmi_mode=1
# hdmi_mode=87
# hdmi_cvt 480 320 60 6 0 0 0
# hdmi_drive=2
# display_rotate=0
```
