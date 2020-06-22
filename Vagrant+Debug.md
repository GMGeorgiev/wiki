Vagrant
===
Vagrant download page:
---
https://www.vagrantup.com/downloads.html

In work directory:
---

>vagrant init

Example VagrantFile:
---

```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.box_check_update = true
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "public_network", bridge: "Intel(R) 82579LM Gigabit Network Connection"
  config.vm.synced_folder "../{project_name}", "/var/www/{project_name}"
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "2024"
  end
   config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update
     sudo apt-get install -y apache2
     sudo systemctl start apache2
     sudo systemctl enable apache2
     sudo apt-get install -y mysql-server
     sudo apt-get install php7.2 php7.2-cli php7.2-common
     sudo touch /etc/apache2/sites-enabled/{project_name}.conf
     sudo echo "<VirtualHost *:80>\nServerName {project_name}.develop\n
     ServerAlias {project_name}.develop\n
     DocumentRoot /var/www/{project_name}/{project_name}/public\n
     <Directory /var/www/{project_name}/{project_name}/public/>\n
                Options Indexes FollowSymLinks MultiViews\n
                AllowOverride All\n
                Order allow,deny\n
                allow from all\n
      </Directory>\n
      ErrorLog /var/www/{project_name}/error.log\n
      CustomLog /var/www/{project_name}/requests.log combined\n
      </VirtualHost>" > /etc/apache2/sites-enabled/{project_name}.conf
     sudo ln -s /etc/apache2/sites-enabled/{project_name}.conf /etc/apache2/sites-available/
     sudo a2enmod rewrite
     sudo a2ensite /etc/apache2/sites-enabled/{project_name}.conf
     sudo service apache2 restart
   SHELL
end

```
After which we use:
---
>vagrant up
 - and
>vagrant provision

- This vagrant file is designed to create a virtual host, with alias **{project_name}.develop** . Change it to whatever you need in the VagrantFile. It also syncs the directory **/{project_name}** in the same directory as the VagrantFile, with **/var/www/{project_name}/** on the vagrant box which is created from the shell scripts in the file. Configure as per your project name

In /etc/apache2/apache2.conf
---
Where DocumentRoot /var/www/ change AllowOverride from **None** to **All**

After vagrant ssh, install xdebug in the vagrant box
---
>sudo apt-get install php-xdebug

Restart Apache
---
>sudo service apache2 restart

To Preview your project locally in Windows add to your hosts file:
---
>192.168.33.10: {project_name}.develop

PHP X-Debug *(for VS Code)*
===

1. Install PHP Debug from the extensions in VS Code
2. Click create launch.json file.
3. Example launch.json file *(as per previously configured VagrantBox):
```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for XDebug",
      "type": "php",
      "request": "launch",
      "port": 9000,
      "pathMappings": {
        "/var/www/{project_name}/{project_name}/": "${workspaceRoot}/{project_name}/"
      }
    },
    {
      "name": "Launch currently open script",
      "type": "php",
      "request": "launch",
      "program": "${file}",
      "cwd": "${fileDirname}",
      "port": 9000,
      "runtimeExecutable": "C\\xampp2\\php\\php.exe"
    }
  ]
}
```
4. Example xdebug.init file (In the Vagrant box):
   1. First run in your project where the VagrantFile lies:
   > vagrant ssh

5. Install Xdebug:
>apt-get install php-xdebug
```
/etc/php/7.2/mods-available/xdebug.ini
zend_extension=xdebug.so
[XDebug]
xdebug.remote_enable=1
xdebug.remote_autostart=1
; Host machine of Vagrant instance
xdebug.remote_host=10.0.60.63
xdebug.remote_connect_back=1
```
Where xdebug.remote_host is Your local IP






