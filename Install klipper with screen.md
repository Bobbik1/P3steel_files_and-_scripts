install  mainsailOs by raspberry pi imager
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
sudo reboot```

voila!

you may lower down SPI a bit
In your /boot/firmware/config.txt, change:
dtoverlay=waveshare35a:rotate=90
to:
dtoverlay=waveshare35a:rotate=90,speed=16000000
