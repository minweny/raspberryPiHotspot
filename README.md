# raspberryPiHotspot
Raspberry Pi 4 Hotspot Set Up / 树莓派4热点设置

## Useful commands:
```bash
sudo shutdown -h now
sudo reboot
exit
ssh pi@raspberrypi.local
ping raspberrypi.local
# ssh to ipv6
ssh pi@fe80::a957:4002:81fa:2c4c%18
```
## Notes:
* nmap, raspberrypi.local tutorial
> [https://www.raspberrypi.org/documentation/remote-access/ip-address.md]

* Resolve "Host key verification failed"
> Delete raspberrypi.local column in known_hosts file if you are using windows  
> For example, C:\Users\ybob\.ssh\known_hosts

## Steps:
1. Download Raspbian[https://www.raspberrypi.org/downloads/raspbian/]  
We choose "Raspbian Buster with desktop and recommended software"

2. Flash Raspbian system to SD card with balenaetcher[https://www.balena.io/etcher/]

3. Use ethernet cable to connect your PC ethernet port to Raspberry Pi ethernet port  
  Looks like this:
    >PC - PC ethernet port - Raspberry Pi ethernet port - Raspberry Pi

    PC ethernet port configuration:  
    >IP address: 192.168.0.1  
    >Subnet mask: 255.255.255.0

4. Boot Raspberry Pi, enable SSH, VNC [https://www.raspberrypi.org/documentation/remote-access/ssh/]  
Open terminal, type 
```
  ifconfig
```

    or

```
  ip a
```

    From eth0 row, write down your pi IP address. My pi shows 192.168.0.151

5. Switch back to your PC, ping pi_ip_address to verify if your connection works
```
  ping pi_ip_address
```

    or

```
  ping raspberrypi.local
```

6. From PC, SSH to your pi
```
  ssh pi@pi_ip_address
```

    or

```
  ssh pi@raspberrypi.local
```

7. Set up static IP[https://www.ionos.com/digitalguide/server/configuration/provide-raspberry-pi-with-a-static-ip-address/]


