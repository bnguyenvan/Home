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
mv -rf ./data/images ./data/images_$(date +"%Y%m%d-%H%M")
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
https://docs.sam.gy/books/bookstack/page/enabling-https-for-the-bookstack-web-interface
