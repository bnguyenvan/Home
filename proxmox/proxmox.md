Proxmox
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
[I. Install Proxmox]

### Auto shutdown Proxmox when lost Internet
Create a script allow system shutdown in the case of lost internet. This case happen when lost electric and UPS could not stand for over 15 minutes, then system will shutdown shutdently cause damaged hardware.
The script will run as a system service on boot.

#### Step 1 - Create a script 
```bash
nano /usr/local/bin/auto_shutdown.sh`
```

Adding content:
```bash
#!/bin/bash

# Log file
LOGFILE="/var/log/auto_shutdown.log"

# Counter for consecutive timeouts
timeout_count=0
# Maximum allowed timeouts before shutdown
max_timeouts=5

# Function to log events
log() {
  echo "$(date): $1" | tee -a "$LOGFILE"
}

log "Auto shutdown script started."

while true; do
  if ! ping -c 1 -W 2 8.8.8.8 > /dev/null; then
    ((timeout_count++))
    log "Ping timeout #$timeout_count"
  else
    timeout_count=0
  fi

  if [ "$timeout_count" -ge "$max_timeouts" ]; then
    log "5 consecutive timeouts. Shutting down..."
    sudo shutdown -h now
    break
  fi

  sleep 1
done
```
Allow script execution:
```bash
chmod +x /usr/local/bin/auto_shutdown.sh
```
#### Step 2 - Create a systemd service file
```bash
nano /etc/systemd/system/auto_shutdown.service
```
Add the following content
```bash
[Unit]
Description=Auto shutdown on ping timeout
After=network.target

[Service]
ExecStart=/usr/local/bin/auto_shutdown.sh
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```
Explanation:
After=network.target ensures the script starts only after the network is up.
Restart=always restarts the script if it crashes.
User=root runs the script with superuser permissions (needed for shutdown).

#### Step 3 - Enable and start the service
```bash
systemctl daemon-reload
systemctl enable auto_shutdown.service
systemctl start auto_shutdown.service
```

Check the service status
```bash
systemctl status auto_shutdown.service
journalctl -u auto_shutdown.service -f
```

