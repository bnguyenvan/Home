NGINX PROXY MANAGER
---

# Install Nginx Proxy Manager
Run bellow script to install Nginx Proxy Manager
```bash
bash -c "$(wget -qLO - https://github.com/bnguyenvan/HomeLab/raw/main/nginxproxymanager/nginxproxymanager.sh)"
```
This will create nginx proxy manager container:
* os: ubuntu 22.04
* cpu: 1
* ram: 1G
* pre-install: certbot==2.6.0,certbot-dns-godaddy==2.6.0,acme==2.6.0.
Note: 
* At the time of this installation, latest certbot version is 2.10, the script will install acme==2.10 and install certbot-dns-godaddy==2.10 but certbot-dns-godaddy latest version is 2.8
* If we install certbot and acme version to 2.8, we will get error:
```
```

Default Account
```bash
Email:    admin@example.com
Password: changeme
```