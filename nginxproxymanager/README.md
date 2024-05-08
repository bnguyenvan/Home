## NGINX PROXY MANAGER
---

### Install Nginx Proxy Manager
Run bellow script to install Nginx Proxy Manager from proxmox shell
```bash
bash -c "$(wget -qLO - https://github.com/bnguyenvan/HomeLab/raw/main/nginxproxymanager/nginxproxymanager.sh)"
```
Choose Advanced during installation.
* debian 12
* Unprivileged
* Set a Bridge: vmbr1
* Enable Root SSH Access: Yes
* Enable Verbose Mode: No

### Install certbot
We have to re-install with version 2.6
```bash
pip install certbot-dns-godaddy==2.6.0
pip install certbot==2.6.0
pip install acme==2.6.0
```

Default Account login to web interface
```bash
Email:    admin@example.com
Password: changeme
```

### Create new Let's Encrypt Certificate
Nginx Proxy Manager > SSL Certificates > Add SSL Certificate > Let's Encrypt
### Godady
* Domain Names: *.ducloi.store
* Email Address for Let's Encrypt: microwave88@gmail.com
* Use a DNS Challenge: enable
* DNS Provider: GoDaddy
* Credentials File Content:
  * dns_godaddy_secret = Your_GoDaddy_Key
  * dns_godaddy_key = Your_GoDaddy_Secret
  * I agree to the [Let's Encrypt Terms of Service*](https://letsencrypt.org/repository/) : enable

### Duckdns
  * Domain Names: *.eawer.ducloi.store
  * Email Address for Let's Encrypt: microwave88@gmail.com
  * Use a DNS Challenge: enable
  * DNS Provider: DuckDNS
  * Credentials File Content:
  * dns_duckdns_token = Your_DuckDNS_Token
  * Propagation Seconds: 120
  * I agree to the [Let's Encrypt Terms of Service*](https://letsencrypt.org/repository/) : enable

### Add new Host
Nginx Proxy Manager > Hosts > Proxy Hosts
* Details:
  * Domain Names: books.ducloi.store
  * Scheme: http
  * Forward Hostname / IP: 192.168.31.200
  * Forward Port: 80
  * Websockets Support: Enable (Optional)
* SSL:
  * SSL Certificate: books.ducloi.store
  * Force SSL: enable
  * HTTP/2 Support: enable

## Trouble Shooting
### Case 1: Could not login to Nginx Proxy Manager although login page appear
Reason: some Host with certificate invalid, when reboot Nginx Proxy Manager, service could not start due to error.
Solution: Check log at ```/var/log/nginx/error.log```. Or you could remove all proxy_host at ```cd /data/nginx/proxy_host/``` and try to restart nginx again:
```bash
root@NginxProxyManager:/var/log/nginx# systemctl status npm
```
