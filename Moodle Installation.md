# Moodle Installation Guide
## Step 1: Install Required Packages

```bash
sudo apt update
sudo apt install apache2 mysql-server php php-cli php-fpm php-mysql php-xml php-mbstring php-curl php-zip php-soap php-intl php-gd php-bcmath git unzip -y
```

---

## Step 2: Configure Database

Login to MySQL:

```bash
sudo mysql -u root -p
```

Create database and user:

```sql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'StrongPassword';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## Step 3: Download Moodle

```bash
cd /var/www/html
sudo git clone https://github.com/moodle/moodle.git
cd moodle
sudo git checkout MOODLE_404_STABLE
```

---

## Step 4: Create Moodle Data Directory

```bash
sudo mkdir /var/moodledata
sudo chown -R www-data:www-data /var/moodledata
sudo chmod -R 775 /var/moodledata
```

---

## Step 5: Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/html/moodle
sudo chmod -R 755 /var/www/html/moodle
```

---

## Step 6: Configure Apache

Create a virtual host:

```bash
sudo nano /etc/apache2/sites-available/moodle.conf
```

Add the following:

```apache
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/moodle

    <Directory /var/www/html/moodle>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/moodle_error.log
    CustomLog ${APACHE_LOG_DIR}/moodle_access.log combined
</VirtualHost>
```

Enable site and rewrite module:

```bash
sudo a2ensite moodle.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---

## Step 7: Complete Installation via Browser

Open your browser and go to:

```
http://your-server-ip/
```

Follow the on-screen instructions:

* Select language
* Confirm paths
* Configure database
* Create admin account

---

## Step 8: Set Up Cron Job

```bash
sudo crontab -u www-data -e
```

Add:

```bash
* * * * * /usr/bin/php /var/www/html/moodle/admin/cli/cron.php >/dev/null
```

---

## Optional: Enable HTTPS

Install Certbot:

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache
```

---

## Troubleshooting

### Common Issues

* Blank page → Check PHP errors/logs
* Permission issues → Verify ownership (www-data)
* DB connection error → Recheck credentials

Logs:

```bash
/var/log/apache2/error.log
```

---

## Conclusion

You have successfully installed Moodle.
