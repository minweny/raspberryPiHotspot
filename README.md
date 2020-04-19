# raspberryPiHotspot
Raspberry Pi 4 Hotspot Set Up / 树莓派4热点设置

1. Download Raspbian[https://www.raspberrypi.org/downloads/raspbian/]  
We choose "Raspbian Buster with desktop and recommended software"

2. Flash Raspbian system to SD card with balenaetcher[https://www.balena.io/etcher/]

3. Use ethernet cable to connect your PC ethernet port to Raspberry Pi ethernet port  
  Looks like this
   >PC - PC ethernet port - Raspberry Pi ethernet port - Raspberry Pi
---:
aa
 PC ethernet port configuration:  
- IP address: 192.168.0.1
- Subnet mask: 255.255.255.0

3. Boot Raspberry Pi, enable SSH, VNC [https://www.raspberrypi.org/documentation/remote-access/ssh/]
Open terminal, type
```
ifconfig
```
or
```
ip a
```
From eth0 row, write down your pi IP address. My pi shows 192.168.0.151

4. Switch back to your PC, ping pi_ip_address to verify if your connection works
```
ping pi_ip_address
```
or
```
ping raspberrypi.local
```

5. From PC, SSH to your pi
```
ssh pi@pi_ip_address
```
or
```
ssh pi@raspberrypi.local
```
```
sudo shutdown -h now
```

6. Set up static IP[https://www.ionos.com/digitalguide/server/configuration/provide-raspberry-pi-with-a-static-ip-address/]


