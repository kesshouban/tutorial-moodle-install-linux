# tutorial-moodle-install-linux
un totorial de la instalacion del moodle en debian 11

## primer paso 
connectar al servidor       

``` powershell 
ssh root@[host] 
```     
## segundo paso
instalar lnmp(Linux, Nginx, MariaDB, PHP) manualmente.     
1. instalamos nginx
```bash
apt update 
apt install nginx 
```

2. 
instalar mariadb
```bash
apt install mariadb-server
```
luego configuramos mysql.
```bash
mysql_secure_installation
```
```bash
root@vultr:~# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
```
configuramos contraseña de root para mysql
```bash
Switch to unix_socket authentication [Y/n] y
Enabled successfully!
Reloading privilege tables..
 ... Success!


You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y

New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
root@vultr:~#
```

3. instalamos php

```bash
apt install php php-fpm php-iconv php-intl php-soap php-zip php-curl php-mbstring php-mysql php-gd php-xml php-pspell php-json php-xmlrpc
```
4. configuramos nginx
```bash
nano /etc/nginx/sites-available/default
```
añadimos los siguiente lineas al final del archivo
```bash
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
} 
```
comprobar la configuracion que esta bien utilizando el commando `nginx -t`
```bash
root@vultr:~# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@vultr:~#
```
reload nginx 
```bash
/etc/init.d/nginx reload
```
creamos `phpinfo.php` para comprobarlo
```bash
nano /var/www/html/phpinfo 
```
y ponemos 
```php
<?php 
phpinfo();
?>
```
si salimos este es decir la configuracion ya es completada
![img]

## Tercer Paso
descargar y configuramos moodle 
```bash
root@vultr:~# wget https://download.moodle.org/download.php/stable401/moodle-latest-401.zip
--2023-01-13 15:58:44--  https://download.moodle.org/download.php/stable401/moodle-latest-401.zip
Resolving download.moodle.org (download.moodle.org)... 2606:4700:10::6816:4051, 2606:4700:10::6816:4151, 2606:4700:10::ac43:1ae9, ...
Connecting to download.moodle.org (download.moodle.org)|2606:4700:10::6816:4051|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘moodle-latest-401.zip’

moodle-latest-401.zip             [ <=>                                              ]  59.78K  --.-KB/s    in 0.001s

2023-01-13 15:58:45 (42.9 MB/s) - ‘moodle-latest-401.zip’ saved [61212]
```
luego ejecutar los commandos en `/var/www/html`
```bash
unzip moodle-latest-401.zip
cd moodle
mv * ../
mv.* ../
```
## cuarto paso 
configuramos moodle
