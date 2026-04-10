# Quick Moodle Installation Guide
---
## Step 1: Create three directories

```bash
sudo mkdir moodle_dir
sudo mkdir moodle_data_dir
sudo chmod -R 775 moodle_data_dir
sudo mkdir phpu_moodle_dir
sudo chmod -R 775 phpu_moodle_dir
```

---

## Step 2: Create a database and grant all privileges to the user:

```sql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## Step 3: Add moodle_dir to git worktree

```bash
cd path_to_moodle git worktree directory
git worktree add path_to_moodle_dir branch
cd path_to_moodle_dir
```

---

## Step 4: Configure Apache

Create a virtual host:

```bash
sudo nano /etc/apache2/sites-available/moodle.conf
```

Add the following if Moodle version is up to 5.0:

```apache
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName moodle_site_name
    DocumentRoot /var/www/html/moodle

    <Directory /var/www/html/moodle>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/moodle_error.log
    CustomLog ${APACHE_LOG_DIR}/moodle_access.log combined
</VirtualHost>
```
Add the following if Moodle version is 5.1+:

```apache
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName moodle_site_name
    DocumentRoot /var/www/html/moodle/public

    <Directory /var/www/html/moodle/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/moodle_error.log
    CustomLog ${APACHE_LOG_DIR}/moodle_access.log combined
</VirtualHost>
```
Add the Moodle_site_name in the hosts file:
```bash
sudo nano /etc/hosts
```

Enable site module:

```bash
sudo a2ensite moodle.conf
sudo systemctl restart apache2
```

---

## Step 5: Complete Installation via Browser

Open your browser and go to:

```
http://moodle_site_name/
```

Follow the on-screen instructions:

* Select language
* Confirm paths
* Configure database
* Create admin account

---

## Conclusion

You have successfully installed Moodle.
