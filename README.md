BOOKSTACK
--------

# Install bookstack

Ubuntu LXC Install

```bash
bash -c "$(wget -qLO - https://github.com/bnguyenvan/HomeLab/raw/main/lxc/ubuntu.sh)"
```

Bookstack Install

```bash
bash -c "$(wget -qLO - https://github.com/bnguyenvan/HomeLab/raw/main/bookstack/bookstack.sh)"
```

# Backup Data
### Backup database
Inside bookstack container:
```bash
mysqldump bookstack > /tmp/bookstack_backup.sql
```

Copy to local pc
```bash
mv ./data/bookstack_backup.sql ./data/bookstack_backup_$(date +"%Y%m%d-%H%M").sql
scp root@CONTAINER_IP_ADDR:/tmp/bookstack_backup.sql ./data/bookstack_backup.sql
```
Delete backup file after copy:
```bash
rm -rf /tmp/bookstack_backup.sql
```

### Backup images data
```bash
mv ./data/images ./data/images_$(date +"%Y%m%d-%H%M")
scp -r root@CONTAINER_IP_ADDR:/var/www/bookstack/public/uploads/images ./data/
```

# Restore Data
### Restore database
Copy backup file from local pc to container:
```bash
scp ./data/bookstack_backup.sql root@CONTAINER_IP_ADDR:/tmp/bookstack_backup.sql
```

Restore backup file:
```bash
mysql -D bookstack < /tmp/bookstack_backup.sql
```

Delete backup file after restore:
```bash
rm -rf /tmp/bookstack_backup.sql 
```

### Restore images data

```bash
scp -r ./data/images root@CONTAINER_IP_ADDR:/var/www/bookstack/public/uploads/
```

# Enabling HTTPS for the BookStack web interface

## Method 1 - Enable https directly on bookstack
STEP 1:
### Create a let's Encrypt SSL certificate on proxmox server
[Create let's encrypt certificate with Certbot](https://books.ducloi.store/books/home-ebook/page/ssl-certificate-create-lets-encrypt-certificate-with-certbot)

### Mount certificate from proxmox server to container
On bookstack container:
```bash
mkdir -r /etc/letsencrypt/live/YOUR-DOMAIN-HERE
mkdir -r /etc/letsencrypt/archive/YOUR-DOMAIN-HERE
```
On Proxmox:
```bash
vim /etc/pve/lxc/YOUR_LXC_ID.conf
```
Added bellow line at the end of file
```bash
mp0: /etc/letsencrypt/archive/YOUR-DOMAIN-HERE,mp=/etc/letsencrypt/archive/YOUR-DOMAIN-HERE
```
Now go back to bookstack container:
```bash
ln -s ../../archive/YOUR-DOMAIN-HERE/fullchain2.pem fullchain.pem
ln -s ../../archive/YOUR-DOMAIN-HERE/privkey2.pem privkey.pem
```
* `fullchain2.pem`: fullchain file mounted from `/etc/letsencrypt/archive/YOUR-DOMAIN-HERE/fullchain2.pem` on proxmox server
* `privkey2.pem`: private key file mounted from `/etc/letsencrypt/archive/YOUR-DOMAIN-HERE/privkey2.pem` on proxmox server

### Modify apache config to using https
Move the original BootStack config file (so you can access it again if needed)
```bash
sudo mv /etc/apache2/sites-available/bookstack.conf /etc/apache2/sites-available/bookstack.conf.old
```

Create a new BookStack Apache2 config file:
```bash
sudo nano /etc/apache2/sites-available/bookstack.conf
```
Here is an example configuration - you will need to change the ServerName and SSLCertificate directives to match your requirements.
```bash
<VirtualHost *:80>
    ServerName YOUR-DOMAIN-HERE
    RewriteEngine On
    RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
</VirtualHost>

<VirtualHost *:443>
	ServerName YOUR-DOMAIN-HERE
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/bookstack/public/

    SSLEngine on
    SSLCertificateFile      /etc/letsencrypt/live/YOUR-DOMAIN-HERE/fullchain.pem
    SSLCertificateKeyFile   /etc/letsencrypt/live/YOUR-DOMAIN-HERE/privkey.pem

    <Directory /var/www/bookstack/public/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
        <IfModule mod_rewrite.c>
            <IfModule mod_negotiation.c>
                Options -MultiViews -Indexes
            </IfModule>
            RewriteEngine On
            # Handle Authorization Header
            RewriteCond %{HTTP:Authorization} .
            RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
            # Redirect Trailing Slashes If Not A Folder...
            RewriteCond %{REQUEST_FILENAME} !-d
            RewriteCond %{REQUEST_URI} (.+)/$
            RewriteRule ^ %1 [L,R=301]
            # Handle Front Controller...
            RewriteCond %{REQUEST_FILENAME} !-d
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteRule ^ index.php [L]
        </IfModule>
    </Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Enable Apache2's SSL module
```bash
sudo a2enmod ssl
```

Run Apache2's built in config tester to confirm there are no errors:
```bash
sudo apachectl configtest
Syntax OK
```

Edit BookStack's .env config file:
```bash
sudo nano /var/www/bookstack/.env
```

## Method 2 - Using Nginx Proxy Manager
Following step [here](https://github.com/bnguyenvan/HomeLab/tree/main/nginxproxymanager)

STEP 2:
Change the APP_URL parameter so that it uses the full domain name, making sure that it specifies https:// not http://
```bash
# Application URL
# This must be the root URL that you want to host BookStack on.
# All URLs in BookStack will be generated using this value
# to ensure URLs generated are consistent and secure.
# If you change this in the future you may need to run a command
# to update stored URLs in the database. Command example:
# php artisan bookstack:update-url https://old.example.com https://new.example.com
APP_URL=https://YOUR-DOMAIN-HERE
```
Restart Apache2
```bash
sudo systemctl restart apache2
```
Ref: [enabling-https-for-the-bookstack-web-interface](https://docs.sam.gy/books/bookstack/page/enabling-https-for-the-bookstack-web-interface)