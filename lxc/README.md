Ubuntu
--------

# Install ubuntu lxc container

Ubuntu LXC Install

```bash
bash -c "$(wget -qLO - https://github.com/bnguyenvan/HomeLab/raw/main/lxc/ubuntu.sh)"
```

# Share folder between proxmox and local host:
Edit file
```bash
vi /etc/pve/lxc/CONTAINER_ID.conf
```
then add bellow line:
```conf
mp0: /source/to/folder,mp=/container/folder
```
It is easiser to set container as Privileged during settup