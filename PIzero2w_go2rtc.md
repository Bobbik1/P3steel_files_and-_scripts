# Streaming CSI Camera on Raspberry Pi Zero 2 W with go2rtc

This guide explains how to set up a low-latency H.264 camera stream using an **OV5647 (v1.3)** CSI camera on **Raspberry Pi OS Bookworm Lite** (64-bit) using **go2rtc**.

## 1. Hardware Connection
Ensure the camera ribbon cable is inserted correctly. On the Pi Zero 2 W, the silver contacts should face **downward** (towards the PCB).

## 2. Enable Camera Driver
In Raspberry Pi OS Bookworm, the legacy camera stack is replaced by `libcamera`. For the OV5647 sensor, you must manually force the overlay.

1. Edit the config file:
   ```bash
   sudo nano /boot/firmware/config.txt
Find the [all] section and add these line:

```shell

dtoverlay=ov5647
```
and comment line :
```shell
#camera_auto_detect=1
```

Save and reboot:

```shell
sudo reboot
```
Verify the camera is detected:

```shell
rpicam-hello --list-cameras
```
3. Install go2rtc
Download the latest ARM64 binary and set permissions:

```shell
wget [https://github.com/AlexxIT/go2rtc/releases/latest/download/go2rtc_linux_arm64](https://github.com/AlexxIT/go2rtc/releases/latest/download/go2rtc_linux_arm64) -O go2rtc
chmod +x go2rtc
```
4. Configuration
Create a go2rtc.yaml file in the same directory:

```shell
nano go2rtc.yaml
```
Add the following configuration (optimized for Pi Zero 2 W performance and WiFi stability):
```shell
YAML
api:
  listen: ":1984"
  origin: "*"

streams:
  kamera_csi:
    - exec:rpicam-vid -t 0 --inline --codec h264 --width 1280 --height 720 --framerate 20 --bitrate 2000000 --nopreview -o -

webrtc:
  listen: ":8555"
  # add static adrress because mainsail can lost
  candidates:
    - 10.236.245.88:8555  # Adress of your Pi Zero
    - lan                 # Automatic local network 
```
Note: Bitrate is limited to 2Mbps to prevent stuttering on Pi Zero's WiFi.

5. Auto-Start with Systemd
Create a service file to run go2rtc on boot:

```shell
sudo nano /etc/systemd/system/go2rtc.service
```
Paste the following (adjust /home/pi/ if your username is different):
```shell
[Unit]
Description=go2rtc service
After=network.target

[Service]
User=pi
WorkingDirectory=/home/pi/
ExecStart=/home/pi/go2rtc
Restart=always
RestartSec=5


[Install]
WantedBy=multi-user.target
```



Enable and start the service:

```shell
sudo systemctl daemon-reload
sudo systemctl enable go2rtc
sudo systemctl start go2rtc
```
6. Accessing the Stream
Open your web browser and navigate to:
```shell
http://<your_pi_ip>:1984
```

WebRTC: Best for low latency (near real-time).

RTSP: rtsp://<your_pi_ip>:8554/csi_cam
