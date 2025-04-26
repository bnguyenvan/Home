# Proxmox

## Information
- Version: 8.1.3
- Hardware:
  - CPU N100 4 cores 4 threads
  - 4xport LAN 2.5G I226V
  - 1xDDR5 32GB
  - 1xHDMI, 1 Displayport
  - 1xM.2 NVME 500GB, 1 SATA SSD Acer 128GB
  - 2 USB 2.0, 2 USB 3.0
  - DC 12V
-------------
## Tips
### Remove no-subscription messages

Run command in Proxmox VE Shell:
```bash
sed -i.backup -z "s/res === null || res === undefined || \!res || res\n\t\t\t.data.status.toLowerCase() \!== 'active'/false/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service
```

Above command will also backup file ```/usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js``` to ```proxmoxlib.js.backup```

Ref: [How to: Remove “You do not have a valid subscription for this server….” from Proxmox Virtual Environment/Proxmox VE (PVE 6.1 to 8.3 and up)](https://dannyda.com/2020/05/17/how-to-remove-you-do-not-have-a-valid-subscription-for-this-server-from-proxmox-virtual-environment-6-1-2-proxmox-ve-6-1-2-pve-6-1-2/)



