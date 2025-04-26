OPNSense
----
# Installation
## Hardware requirements:
   - CPU: 2x2
   - Memory: 4GB
   - Hard Disk: 40GB
   - Network:
     - net0: vmbr0 (MANAGEMENT)
     - net1: vmbr1 (LAN)
     - net2: vmbr2 (WAN)
## OS
    - OPNSense 24.1 dvd amd64


# General Settings
## User setting
- Change password: OpnSense > System > Access > Users
- Enable SSH: OpnSense > System > Settings > Secure Shell:
  * Secure Shell Server: enable
  * Root Login: enable
  * 

# Interface Settings
## LAN
## WAN
## WIFIGUEST
## MANAGEMENT

# OPEN VPN Settings
[OPENVPN Client To Site on OPNSense Firewall](https://books.ducloi.store/books/home-ebook/page/openvpn-config-client-to-site-vpn)

# Dynamic DNS Settings

# DNS Setting

# Backup And Restore
## Backup Config
OPNSense > System > Backups > Download
* Do not backup RRD data: enable
* Encrypt this configuration file: enable
* Password: Firewall Password
* Download Configuration
Save backup file to ```~/github/HomeLab/data```

## Restore Config
OPNSense > System > Backups > Restore
* Restore areas: All (recommended)
* Choose File: ```~/github/HomeLab/data```
* Reboot after a successful restore: enable
* Exclude console settings from import: enable
* Configuration file is encrypted: enable
* Password: Firewall Password
* Restore configuration
