NGINX PROXY MANAGER
---
# Create a LXC container

# Install Nginx Proxy Manager
Run bellow script to install Nginx Proxy Manager inside previous created container
```bash
sh -c "$(wget --no-cache -qO- https://raw.githubusercontent.com/ej52/proxmox/main/install.sh)" -s --app nginx-proxy-manager
```

Default Account
```bash
Email:    admin@example.com
Password: changeme
```