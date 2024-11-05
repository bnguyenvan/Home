curl -L https://mirrors.apqa.cn/proxmox/debian/pveport.gpg | tee /usr/share/keyrings/pveport.gpg


echo "deb [deb=arm64 signed-by=/usr/share/keyrings/pveport.gpg] https://mirrors.apqa.cn/proxmox/debian/pve bookworm port" | tee /etc/apt/sources.list.d/pveport.list

nano /etc/hosts
```shell
127.0.0.1       localhost
127.0.1.1       raspberrypi
192.168.31.220  raspberrypi
```

nano /etc/network/interfaces
```shell
auto lo
iface lo inet loopback

iface wlan0 inet static

auto vmbr0
iface vmbr0 inet static
    address 192.168.31.220/24
    gateway 192.168.31.1
    bridge-ports wlan0
    bridge-stp off
    bridge-fd 0
```

apt install ifupdown2 bridge-utils