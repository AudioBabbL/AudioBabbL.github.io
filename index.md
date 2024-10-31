# Push local changes to this site:
(set up SHH on local machine and Github for password-less operation) -- see: https://thelinuxcode.com/push-to-github-without-password-using-ssh-key/

from local directory:
```
git add --all &&
git commit -m "Initial commit" &&
git push -u origin main
```

# --- 2024-10-30 ---

## setup LAMP: Linux, Apache2, MariaDB, PhpMyAdmin

- **Linux:**  Debian 12 on Hostinger KVM-1

- **Apache-2:**       [see https://phoenixnap.com/kb/how-to-install-phpmyadmin-on-debian]

```
sudo apt update -y
```

```
sudo apt install wget -y
```

```
sudo apt install apache2 -y
```

```
systemctl status apache2
```

```
sudo apt -y install php php-cgi php-mysqli php-pear php-mbstring libapache2-mod-php php-common php-phpseclib php-mysql
```

```
php --version
```

- **MariaDB:**

```
sudo apt install mariadb-server mariadb-client -y
```

```
systemctl status mariadb
```

```
sudo mysql_secure_installation
```
As you have not yet set a root password for your database, hit Enter to skip the initial query. Complete the following queries:

Switch to unix_socket authentication [Y/n]. Enter n to skip.

Set root password? [Y/n]. Type y and press Enter to create a strong root password for your database. If you already have a root password, enter n to answer the Change the root password question.

Remove anonymous users? [Y/n]. Type y and press Enter.

Disallow root login remotely? [Y/n]. Type y and press Enter.

Remove test database and access to it? [Y/n]. Type y and confirm with Enter.

Reload privilege tables now? [Y/n]. Type y and confirm with Enter.

- **Create new MariaDB user** [see: https://phoenixnap.com/kb/how-to-create-mariadb-user-grant-privileges]

```
sudo mysql -u root -p
```

```
CREATE DATABASE 'yourDB';
```

```
SHOW DATABASES;
```

```
CREATE USER 'user1'@localhost IDENTIFIED BY 'password1';
```

```
SELECT User FROM mysql.user;
```

```
GRANT ALL PRIVILEGES ON *.* TO 'user1'@localhost IDENTIFIED BY 'password1';
```

```
GRANT ALL PRIVILEGES ON 'yourDB'.* TO 'user1'@localhost;
```

```
FLUSH PRIVILEGES;
```

```
SHOW GRANTS FOR 'user1'@localhost;
```

- **Install phpMyadmin**

```
wget -P Downloads https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
```

```
wget -P Downloads https://files.phpmyadmin.net/phpmyadmin.keyring
```
```
cd Downloads
```
(may need to install gpg)
```
sudo apt-get install gnupg
```
```
gpg --import phpmyadmin.keyring
```
```
wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz.asc
```
```
gpg --verify phpMyAdmin-latest-all-languages.tar.gz.asc
```

```
sudo mkdir /var/www/html/phpMyAdmin
```

```
sudo tar xvf phpMyAdmin-latest-all-languages.tar.gz --strip-components=1 -C /var/www/html/phpMyAdmin
```


```
sudo cp /var/www/html/phpMyAdmin/config.sample.inc.php /var/www/html/phpMyAdmin/config.inc.php
```
```
sudo nano /var/www/html/phpMyAdmin/config.inc.php
```
enter blowfish passphrase of choice, ctrl+X, Y

```
sudo chmod 660 /var/www/html/phpMyAdmin/config.inc.php
```
```
sudo chown -R www-data:www-data /var/www/html/phpMyAdmin
```
```
sudo systemctl restart apache2
```

access phpMyadmin from browser: <<localhost>>/phpMyAdmin

use 'root' and password set during MariaDB secure setup.


# --- 2024-10-31 ---

## setup nameservers / DNS

- point DNS records to IP of VPS
- two A records: [1] domain.com [2] www.domain.com

check with:

```
dig A +short domain.com
```

## setup ufw firewall

```
sudo apt install ufw
```
```
sudo ufw enable
```

```
sudo ufw app list
```
```
sudo ufw allow in "WWW Full"
```
```
sudo ufw allow in "OpenSSH"
```

```
sudo ufw status
```
(should look like this:)
```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
WWW Full                ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
WWW Full (v6)           ALLOW       Anywhere (v6)        
```

- **Create apache hostfile config**

```
nano /etc/apache2/sites-available/yourdomain.conf
```

```
<VirtualHost *:80>
    ServerName example.com
    # ServerAlias www.example.com
    ServerAdmin webmaster@example.com
    DocumentRoot /var/www/wordpress

    <Directory /var/www/wordpress>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/wordpress-error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress-access.log combined
</VirtualHost>
```

create sybolic link:
```
ln -s /etc/apache2/sites-available/yourdomain.conf /etc/apache2/sites-enabled/
```
```
a2ensite yourdomain.conf
```
```
apachectl configtest
```
```
systemctl restart apache2
```


## setup permanent ssl cert

Let's Encrypt & Certbot

```
sudo apt update -y && sudo apt install certbot python3-certbot-apache
```

```
sudo systemctl stop apache2
```
```
sudo certbot --apache
```
TEST:
```
sudo certbot renew --dry-run
```

```
sudo systemctl start apache2
```


## setup WORDPRESS

- **create database / user**

via phpMyAdmin: create database, create user, grant all priveleges to that database to user

- **Download Wordpress**

```
cd /tmp
```
```
wget https://wordpress.org/latest.tar.gz
```
```
tar xpf latest.tar.gz
```
(if not done already)
```
mkdir /var/www/html/youdomainfolder
```
```
cp -R wordpress/* /var/www/html/yourdomainfolder/
```

```
chown -R www-data:www-data /var/www/html/yourdomainfolder
```
```
find /var/www/html/yourdomainfolder -type d -exec chmod 755 {} \;
```
```
find /var/www/html/yourdomainfolder -type f -exec chmod 644 {} \;
```

