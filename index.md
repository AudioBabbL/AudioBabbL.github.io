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