
## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
### Deploying a LAMP Stack Web Application on AWS Cloud
A LAMP stack is a bundle of four different software technologies that developers use to build websites and web applications. LAMP is an acronym for the operating system, Linux; the web server, Apache; the database server, MySQL; and the programming language, PHP.
### Creation of EC2 Instance
First I log on to AWS Cloud Services and create an EC2 Ubuntu VM instance. When creating an instance, I choose keypair authentication and download private key(ben.pem) on my local computer.
![ec2in](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/7666c0bc-7719-4889-8dc7-885ed834ea39)
On windows terminal, I cd into the directory containing the downloaded private key. and i run the below command to log into the instance via ssh:

ssh -i ben.pem ubuntu@172-31-14-87
![ubuntuconsole](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/81d61a07-4a3e-4c34-9755-460166836442)
### Setting Up Apache Web Server
To deploying the web application,  i installed apache via ubuntu package manager apt:
#Updating Packages
$ sudo apt update
$ sudo apt install apache2
![apaches](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/914788bc-c779-4acd-a90f-16b9d86f6c40)
#starting apache2 Server
$ systemctl start apache2

#ensuring apache2 starts automatically on system boot
$ systemctl enable apache2

#checking server spunned
$ systemctl status apache2
![apachestatus1](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/2f4ee343-e6e1-4ac9-9b8a-8a0993eab85d)
### Configuring Security Group Inbound Rules on EC2 Instance
A Security group is a group of rules that acts as a virtual firewall to the type of traffic that enters (inbound traffic) or leaves (outbound traffic) an instance.

When the instance is created, i have a default TCP rule on port 22 opened which is useful for SSH connection to a terminal. In order to ensure that the webpage are being acccessed on the internet, i opened a TCP port 80 inbound rule.
![http](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/6e0e3e25-f26f-40cd-87a4-66b534585944)
To check the accessiblity of our web server on the internet, i curl the IP address/DNS name of our localhost.

curl http://127.0.0.1:80  or curl http://localhost:80
To see if our web application server can respond to requests , i use the public ip address of the instance on a web browser. http://18.118.33.67:80
![image](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/0f689b87-12c3-4dc0-bc60-4cd5bd7c8aa7)
### Installing MySQL
I installed mysql using the sudo apt install mysql command.
![mysqlinstall](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/4c6bfa5c-5f83-4d1e-a37f-005a191b278d)
From >mysql
I entered ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
I exit the MySQL shell
I started the interactive script by running: sudo mysql_secure_installation
![sql1](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/56a81814-762b-4b7f-ba51-dbd958788b11)
On successful secure configuration, i entered sudo mysql -p on the terminal to have access to the MySQL DB.
After entering the mysql DB, i exit with command Exit
### Installing PHP and its Modules
I installed php alongside its modules, php-mysql which is php module that allows php to communicate with the mysql database, libapache2-mod-php which ensures that the apache web server handles the php contents properly with the code below.

sudo apt install php php-mysql libapache2-mod-php.
![php](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/15076d3a-9f19-41b9-b0bc-f57c3b21fc1b)
On successfull installation of php and its modules I checked the version to see if it was properly installed.
![phpv](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/297c7cc3-c664-4f1a-ae56-600cd81c739f)
### Deploying Our Site on Apache's VirtualHost
Creating Web Domain For the Site
Apache webserver serves a website by the way of server blocks inside its /var/www/ directory, and it can support multiple of this server blocks to host other websites.

I created a new directory called florintechlampstack inside the /var/www/ directory.
sudo mkdir /var/www/florintechlampstack

The I changed permissions of the florintechlampstack directory to the current user system

sudo chown -R $USER:$USER /var/www/florintechlampstack
Then i created and open a new configuration file in Apache’s sites-available directory using sudo nano /etc/apache2/sites-available/florintechlampstack.conf, then i paste the text below
<VirtualHost *:80>
    ServerName florintechlampstack
    ServerAlias www.florintechlampstack
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/florintechlampstack
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Then type ctrl + o to save and ctrl + x to exit the editor
I run sudo a2ensite projectlampstack to activate the server block.
I run sudo a2dissite 000-default to deactivate the default webserver block that comes with apache on default.
then i reload the apache2 server sudo systemctl reload apache2
The new website is now active, but the web root /var/www/florintechlampstack is still empty. I create an index.html file in that location so that i can test that the virtual host works as expected:

sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
Now go to your browser and try to open your website URL using IP address:
http://18.118.33.67:80
![echo](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/fb99ae55-93ee-49d4-b32b-1f1845b04d1a)
###  ENABLE PHP ON THE WEBSITE
I editted the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive using the command: sudo nano /etc/apache2/mods-enabled/dir.conf

After saving and closing the file, i reload Apache so the changes take effect:

sudo systemctl reload apache2

I created a new file named index.php inside the custom web root folder:

nano /var/www/florintechlampstack/index.php

I added the following files which is valid PHP code, inside the file:

'<?php'
'phpinfo();'

Then i saved and close and I refreshed the http://18.118.33.67:80

![phpveon](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/857f0951-f218-4c5e-a245-87b4bda06971)




