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
Ref: [enabling-https-for-the-bookstack-web-interface](https://docs.sam.gy/books/bookstack/page/enabling-https-for-the-bookstack-web-interface)