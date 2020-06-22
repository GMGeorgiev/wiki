1.Install Oracle
===
> https://www.virtualbox.org/

2.Download Ubuntu server
=== 
> https://ubuntu.com/download/server

>Install ubuntu server and change network adapter in the oracle settings to **bridge**

3.Install LAMP
===

Update the Ubuntu:
---

```
sudo apt-get update
```
Install Apache:
---
```
sudo apt-get install apache2
sudo service apache2 status
```
Install MySQL:
---
```
sudo apt-get install mysql-server
```
Install PHP and restart Apache
---
```
sudo apt-get install php libapache2-mod-php php-mysql
sudo systemctl restart apache2 
```
Test PHP Processing on Web Server:
---
```
sudo nano /var/www/html/info.php
```
>Inside the info.php:
```
<?php
phpinfo ();
?>
```
**Open in http://your_ip_address/info.php**

4.Setup Apache Virtual Host:
===

Create directory(if there is no directory created before hand):
---
```
sudo mkdir /var/www/example.com
```
Create the Virtual Hosts File:
---

>Copy the virtual host file(change example to match your wanted domain name)
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```
>Open the virtual host file:
```
sudo vim /etc/apache2/sites-available/example.com.conf
```

>The file should look like(change example with your directory name):

```
<VirtualHost *:80>
 ServerAdmin webmaster@localhost
 DocumentRoot /var/www/example.com
 ServerName example.com
 ServerAlias www.example.com
 ErrorLog ${APACHE_LOG_DIR}/error.log
 CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Enable Virtual Host:
---

```
sudo a2ensite example.com.conf
sudo service apache2 reload
```

>Add the following line to hosts.txt in 
**C:\Windows\System32\drivers\etc\hosts** to the bottom of the page:
```
your_ip_address     your_domain_name
```

5.Shared folder
===
In Windows:
---

>Create a a folder in your Windows you would want to share.

>In Oracle-Settings on your virtual machine click on shared folders and add your Windows shared folder.

In Ubuntu:
---

>Create a folder you want to share in /media/ and run:
```
sudo mkdir /media/{your_folder_name}
sudo mount -t vboxsf {windows-folder-name} /media/{ubuntu-folder-name}
```


