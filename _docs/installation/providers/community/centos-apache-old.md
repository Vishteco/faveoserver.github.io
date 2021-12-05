---
layout: single
type: docs
permalink: /docs/installation/providers/community/centos-apache/
redirect_from:
  - /theme-setup/
last_modified_at: 2020-06-09
toc: true
---

# Installing Faveo Helpdesk Community on Cent OS <!-- omit in toc -->


<img alt="Cent OS Logo" src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Centos-logo-light.svg/300px-Centos-logo-light.svg.png" width="200"  />

Faveo can run on [Cent OS 7](https://www.centos.org/download/).

-   [Prerequisites](#prerequisites)
-   [Installation steps](#installation-steps)
    -   [1. Upload Faveo](#1-upload-faveo)
    -   [2. Setup the database](#2-setup-the-database)
    -   [3. Configure Apache webserver](#5-configure-apache-webserver)
    -   [4. Install Faveo](#3-gui-faveo-installer)
    -   [5. Configure cron job](#4-configure-cron-job)
    -   [6. Redis Installation](#redis-installation)
    -   [7. SSL Installation](#ssl-installation)
    -   [8. Final step](#final-step)


<a id="prerequisites" name="prerequisites"></a>
## Prerequisites

Faveo depends on the following:

-   **Apache** (with mod_rewrite enabled) 
-   **PHP 7.2+** with the following extensions: curl, dom, gd, json, mbstring, openssl, pdo_mysql, tokenizer, zip
-   **MySQL 5.7+** or **MariaDB 10.3+**

### a. LAMP Installation
Follow the [instructions here](https://github.com/teddysun/lamp)
If you follow this step, no need to install Apache, PHP, MySQL separetely as listed below

Login as root user by typing the command below

```sh
sudo su
```

### b. Update your Packages and install some utility tools

```sh
yum update -y && yum install unzip wget nano yum-utils curl openssl git -y
```

###  c. Install and enable additional repositories that contains packages required for Faveo.

```sh
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm 
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm 
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm 
```
###  d. Apache
Install and Enable Apache server.

```sh
yum install httpd -y
systemctl start httpd
systemctl enable httpd
```

### f. PHP 7.2+

Install php 7.2 with these extensions:

```sh
yum-config-manager --enable remi-php72
yum -y install php php-cli php-common php-fpm php-gd php-mbstring php-pecl-mcrypt php-mysqlnd php-odbc php-pdo php-xml  php-opcache php-imap php-bcmath php-ldap php-pecl-zip php-soap
```

<b>Setting Up ionCube</b>
```sh
wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xfz ioncube_loaders_lin_x86-64.tar.gz
```
Copy Ioncube loader to PHP Modules Directory.

```sh
php -i | grep extension_dir
cp ioncube/ioncube_loader_lin_7.2.so /usr/lib64/php/modules 
sed -i '2 a zend_extension = "/usr/lib64/php/modules/ioncube_loader_lin_7.2.so"' /etc/php.ini
sed -i "s/max_execution_time = .*/max_execution_time = 300/" /etc/php.ini
```

### g. Mysql

The official Faveo installation uses Mysql as the database system and **this is the only official system we support**. While Laravel technically supports PostgreSQL and SQLite, we can't guarantee that it will work fine with Faveo as we've never tested it. Feel free to read [Laravel's documentation](https://laravel.com/docs/database#configuration) on that topic if you feel adventurous.

Install Mysql 5.7. Note that this only installs the package, but does not setup Mysql. This is done later in the instructions:

```sh
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm 
yum install mysql-server -y
systemctl start mysqld
systemctl enable mysqld
```

Secure your MySql installation by executing the below command. Set Password for mysql root user, remove anonymous users, disallow remote root login, remove the test databases and finally reload the privilege tables.

```sh
mysql_secure_installation 
```

**phpMyAdmin(Optional):** Install phpMyAdmin. This is optional step. phpMyAdmin gives a GUI to access and work with Database

```sh
yum install phpmyadmin
```
At this point run the belove command to clear the yum cache.
```sh
yum clean all
```

<a id="installation-steps" name="installation-steps"></a>
## Faveo Installation steps

Once the softwares above are installed:


<a id="1-upload-faveo" name="1-upload-faveo"></a>
### 1. Upload Faveo

You may install Faveo by simply cloning the repository. In order for this to work with Apache, you need to clone the repository in a specific folder:

```sh
mkdir -p /var/www/faveo/
cd /var/www/faveo
git clone https://github.com/ladybirdweb/faveo-helpdesk.git
```

You should check out a tagged version of Faveo since `master` branch may not always be stable. Find the latest official version on the [release page](https://github.com/ladybirdweb/faveo-helpdesk/releases)


<a id="2-setup-the-database" name="2-setup-the-database"></a>
### 2. Setup the database

Log in with the root account to configure the database.

```sh
mysql -u root -p
```

Create a database called 'faveo'.

```sql
CREATE DATABASE faveo;
```

Create a user called 'faveo' and its password 'strongpassword'.

```sql
CREATE USER 'faveo'@'localhost' IDENTIFIED BY 'strongpassword';
```

We have to authorize the new user on the faveo db so that he is allowed to change the database.

```sql
GRANT ALL ON faveo.* TO 'faveo'@'localhost';
```

And finally we apply the changes and exit the database.

```sql
FLUSH PRIVILEGES;
exit
```

<a id="5-configure-apache-webserver" name="5-configure-apache-webserver"></a>
### 3. Configure Apache webserver

**a.** Give proper permissions to the project directory by running:

```sh
chown -R apache:apache /var/www/faveo
cd /var/www/faveo
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
```
By default SELINUX will be Enforcing run the follwing comand to switch it to Permissive mode and restart the machine once in order to take effect.
```sh
sed -i 's/SELINUX=enforcing/SELINUX=permissive/g' /etc/selinux/config
reboot -f
```


**b.** Enable the rewrite module of the Apache webserver:

Check whether the Module exists in Apache modules directory.

```sh
ls /etc/httpd/modules | grep mod_rewrite
```
Check if the module is loaded
```sh
grep -i LoadModule /etc/httpd/conf/httpd.conf | grep rewrite
```
If the output af the above command is blank then add the below line in /etc/httpd/conf/httpd.conf

```sh
LoadModule rewrite_module modules/mod_rewrite.so
```

Finally change the httpd.conf AllowOverride value to none to All under <Directory /var/www/> section.
```sh
<Directory "/var/www">
    AllowOverride All 
    # Allow open access:
    Require all granted
</Directory>
```


**c.** Configure a new faveo site in apache by doing:

```sh
nano /etc/httpd/conf.d/faveo.conf
```

Then, in the `nano` text editor window you just opened, copy the following - swapping the `**YOUR IP ADDRESS/DOMAIN**` with your server's IP address/associated domain:

```apache

<VirtualHost *:80> 
ServerName **YOUR IP ADDRESS/DOMAIN** 
ServerAdmin webmaster@localhost 
DocumentRoot /var/www/faveo/
<Directory /var/www/faveo> 
AllowOverride All 
</Directory> 
ErrorLog /var/log/httpd/faveo-error.log 
CustomLog /var/log/httpd/faveo-access.log combined
</VirtualHost>
```

**d.** Apply the new `.conf` file and restart Apache. You can do that by running:

```sh
systemctl restart httpd.service
```


<a id="3-gui-faveo-installer" name="3-gui-faveo-installer"></a>
### 4. Install Faveo

Now you can install Faveo via [GUI](/docs/installation/installer/gui) Wizard or [CLI](/docs/installation/installer/cli)

<a id="4-configure-cron-job" name="4-configure-cron-job"></a>
### 5. Configure cron job

Faveo requires some background processes to continuously run. 
Basically those crons are needed to receive emails
To do this, setup a cron that runs every minute that triggers the following command `php artisan schedule:run`.

Create a new `/etc/cron.d/faveo` file with:

```sh
echo "* * * * * apache /usr/bin/php /var/www/faveo/artisan schedule:run 2>&1" | sudo tee /etc/cron.d/faveo
```


<a id="redis-installation" name="redis-installation"></a>
### 6. Redis Installation

Redis is an open-source (BSD licensed), in-memory data structure store, used as a database, cache and message broker.

This is an optional step and will improve system performance and is highly recommended.

[Redis installation documentation](/docs/installation/providers/enterprise/centos-redis)

<a id="ssl-installation" name="ssl-installation"></a>
### 7. SSL Installation

Secure Sockets Layer (SSL) is a standard security technology for establishing an encrypted link between a server and a client. Let's Encrypt is a free, automated, and open certificate authority.

This is an optional step and will improve system security and is highly recommended.

[Let’s Encrypt SSL installation documentation](/docs/installation/providers/enterprise/centos-apache-ssl)

<a id="final-step" name="final-step"></a>
### 8. Final step

The final step is to have fun with your newly created instance, which should be up and running to `http://localhost` or the domain you have configured Faveo with.