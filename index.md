```
git add --all &&
git commit -m "Initial commit" &&
git push -u origin main
```

--- 2024-10-30 ---

## setup LAMP: Linux, Apache2, MariaDB, PhpMyAdmin

**Linux:**  Debian 12 on Hostinger KVM-1

**Apache-2:**       [see https://phoenixnap.com/kb/how-to-install-phpmyadmin-on-debian]

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

**MariaDB:**

