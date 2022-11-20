# VrGlassViaRpi
## Purpose of the system
To create 3D glasses that can display 3D content with a 3D effect. 

## How
Using Gear VR Optic System from Samsung, two 1.3 inch LCD Displays and two RPI Zero W to realize a VR system.
### Architecture
![Alt-Text](/doc/pic/vrGlassViaRpiArchitecture.png "VrGlassViaRpi - Architecture")

## Motivation 
My motivation was to learn how to build such a system and to reuse parts or the whole project for following projects.

## Install Instructions
### Rpi Zero W
>sudo apt update
>sudo apt upgrade
>sudo apt-get install cmake git

>[all]
>#dtoverlay=vc4-fkms-v3d
>gpu_mem=256
>dtoverlay=w1-gpio

>cd ~
>git clone https://github.com/juj/fbcp-ili9341.git
>cd fbcp-ili9341
>mkdir build
>cd build
>cmake -DST7789VW=ON -DGPIO_TFT_DATA_CONTROL=25 -DGPIO_TFT_RESET_PIN=27 ->DGPIO_TFT_BACKLIGHT=24 -DSINGLE_CORE_BOARD=ON -DBACKLIGHT_CONTROL=ON ->DDISPLAY_CROPPED_INSTEAD_OF_SCALING=ON -DSTATISTICS=0 -DSPI_BUS_CLOCK_DIVISOR=8 ->DDISPLAY_ROTATE_180_DEGREES=OFF ..

>make
>sudo ./fbcp-ili9341

#### modify the config.txt options
>sudo nano /boot/config.txt

>hdmi_force_hotplug=1
>hdmi_cvt=240 240 60 1 0 0 0
>hdmi_group=2
>hdmi_mode=87
>display_rotate=3
>
>Info: clockwise
>#display_rotate:
>#0 =   0°
>#1 =  90°
>#2 = 180°
>#3 = 270°
>....
>
>....
>[pi4]
># Enable DRM VC4 V3D driver on top of the dispmanx display stack
>#dtoverlay=vc4-fkms-v3d
>#max_framebuffers=2

#### optional - Launching the display driver at startup with systemd
>cd /home/pi/fbcp-ili9341/build/
>sudo  mv fbcp-ili9341 fbcp
>sudo cp fbcp /usr/local/sbin
>sudo install -m 0644 -t /etc fbcp.conf 
>sudo install -m 0755 -t /etc/systemd/system fbcp.service 
>modify fbcp.service and change all names from  fbcp-ili9341 to fbcp
>sudo systemctl daemon-reload
>sudo systemctl enable fbcp && sudo systemctl start fbcp
