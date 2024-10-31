```
git add --all &&
git commit -m "Initial commit" &&
git push -u origin main
```

--- 2024-10-30 ---

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
wget -P Downloads https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-english.tar.gz
```