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

# Reset password of LXC
Login to shell of Proxmox and enter bellow commands
```bash
root@pve:~# pct enter <CTID>
```
Ex: If bookstack lxc has id 8004, then type:
```bash
root@pve:~# pct enter 8004
root@bookstack:~#
```
After this, you has been inside container, then just need to change password by type
```bash
root@bookstack:~# passwd
New password: 
Retype new password: 
```
