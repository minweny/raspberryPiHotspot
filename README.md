# raspberryPiHotspot
Raspberry Pi 4 Set Up Process and Launch Hotspot / 树莓派4设置以及创建热点

## Useful commands:
```bash
sudo shutdown -h now
sudo reboot
exit
ping raspberrypi.local
# ping ipv4
ping raspberrypi.local -4
ssh pi@raspberrypi.local
# ssh ipv4 [https://michele.blog/force-ssh-over-ipv4-or-ipv6/]
ssh -4 pi@raspberrypi.local
# ssh to ipv6
ssh pi@fe80::a957:4002:81fa:2c4c%18
# Switch to root privileges
sudo -i
# system log
tail -100f /var/log/syslog
sudo iptables -L
```
## Notes:
* Pi 4 won't boot without HDMI plugged in
> Adding hdmi_force_hotplug=1 to /boot/config.txt seems to have solved the problem. [https://www.raspberrypi.org/forums/viewtopic.php?t=253312]

* nmap, raspberrypi.local tutorial
> [https://www.raspberrypi.org/documentation/remote-access/ip-address.md]

* Resolve "Host key verification failed"
> Delete raspberrypi.local column in known_hosts file if you are using windows  
> For example, C:\Users\ybob\.ssh\known_hosts

* HDMI port
> Raspberry Pi 4 uses Micro HDMI  
> Raspberry Pi Zero uses Mini HDMI

* X-forwarding
> [https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md]

* When PC reboots, pi may lose network
> disable Wifi card, ethernet card, Wifi card sharing. Then reopen.

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
    
    If your PC is using Wifi, right click -> properites -> sharing -> allow other users -> Home networking connection(Ethernet, the one connects with pi). Now, your pi would have the network. 

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
```
sudo nano /etc/dhcpcd.conf
```
append
```
interface eth0
static ip_address=192.168.0.10/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1
```
then
```
sudo reboot
```
verify by
```
ping raspberrypi.local -4
```

8. Install Chinese Input (optional)
```
// Install Google Pinyin input method
sudo apt-get install fcitx fcitx-googlepinyin fcitx-module-cloudpinyin fcitx-sunpinyin
// Restart the Raspberry Pi
reboot
// Use shift to change input method
```

9. Launch Hotspot [https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md]  
To be continued...

## Additonal Notes:
```
配置Pycharm环境变量：
假设你把pycharm安装到/home/pi/pycharm-community-2019.1.3
那么在~/.profile里加一句话
export PATH=$PATH:~/pycharm-community-2019.1.3/bin
保存，然后打开一个新命令行
输入：
pycharm.sh
程序就会启动啦。当命令行关闭时，pycharm会被强制关掉
注：~/.bash_rc似乎没法使用，不知道为啥
```
Adding A 2nd Fixed IP Address To The Ethernet Port
```
For example, say you want the RPi Ethernet port to use DHCP, but to also have a fixed IP address so you can connect to it using a direct cable connection from a laptop.


sudo nano /etc/network/interfaces
Use this for the eth0 adaptor:


auto eth0
allow-hotplug eth0
iface eth0 inet dhcp

auto eth0:1
iface eth0:1 inet static
       address 192.168.1.123
       netmask 255.255.255.0
```
route ip packet from wifi to ethernet [https://www.raspberrypi.org/forums/viewtopic.php?t=223295]
```
Install dnsmasq.
Code: Select all

sudo apt-get install dnsmasq
Assign eth0 an IP address. Open /etc/dhcpcd.conf with a text editor and add this at the bottom.
Code: Select all

interface eth0
static ip_address=192.168.4.1/24
Save original dnsmasq.conf
Code: Select all

sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
Open /etc/dnsmasq.conf with a text editor and add this
Code: Select all

interface=eth0
dhcp-range=192.168.4.8,192.168.4.250,255.255.255.0,12h
Edit /etc/sysctl.conf and uncomment
Code: Select all

net.ipv4.ip_forward=1
Open /etc/rc.local with a text editor and add this just above "exit 0"
Code: Select all

iptables -t nat -A  POSTROUTING -o wlan0 -j MASQUERADE
Reboot.

If you have problems with the dhcp service, you can check the status of dnsmasq.conf.
Code: Select all

sudo service dnsmasq status
It should show active (running), and a list of IP assignments to each mac address..
```
