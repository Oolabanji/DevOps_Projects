## Deploying a LEMP Stack Application On AWS Cloud
### Creation of EC2 Instance
First I log on to AWS Cloud Services and create an EC2 Ubuntu VM instance. When creating an instance, I choose keypair authentication and download private key(ben.pem) on my local computer.
![LEMP](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/78363c41-bb09-4e10-8425-5627ac4e7f36)
On windows terminal, I cd into the directory containing the downloaded private key. and i run the below command to log into the instance via ssh:

ssh -i ben.pem ubuntu@172-31-0-32
![LEMP1](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/a8ba282d-a873-467a-a124-ac4528282013)
### INSTALLING THE NGINX WEB SERVER
$sudo apt update
$sudo apt install nginx
![nginx](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/a95439eb-a56e-4e66-85a8-d0c13f8a045d)
To verify that nginx was successfully installed and is running as a service in Ubuntu, i run:
$sudo systemctl status nginx
![nginx2](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/3d10d3bc-9ab3-4f1c-adbd-37908f82d7e6)
Then  to test how the Nginx server can respond to requests from the Internet. I open a web browser and try to access url http://3.142.201.71🈵
![nginx2](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/b88897c2-8feb-4194-9dee-fc4716635c5b)
###  INSTALLING MYSQL
$ sudo apt install mysql-server
$ sudo mysql
![mysql](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/bc890118-c876-4016-875e-5779e70605f4)
From >mysql
I entered ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
I exit the MySQL shell
I started the interactive script by running: sudo mysql_secure_installation
On successful secure configuration, i entered sudo mysql -p on the terminal to have access to the MySQL DB.
After entering the mysql DB, i exit with command Exit
### Installing PHP and its Modules
I have Nginx installed to serve the content and MySQL installed to store and manage data. Now i installed PHP to process code and generate dynamic content for the web server.
sudo apt install php-fpm php-mysql
![php](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/4d3f706d-0551-47b4-8120-2e4177b9ce64)
### CONFIGURING NGINX TO USE PHP PROCESSOR
I created the root web directory for florintechlemp as follows:
sudo mkdir /var/www/florintechlemp
sudo chown -R $USER:$USER /var/www/florintechlemp
Then I open a new configuration file in Nginx’s sites-available directory using nano.
sudo nano /etc/nginx/sites-available/florintechlemp
This created a new blank file. And I paste in the following bare-bones configuration:

#/etc/nginx/sites-available/florintechlemp

server {
    listen 80;
    server_name florintechlemp www.florintechlemp;
    root /var/www/florintechlemp;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
Enter Ctrl +O to save and Ctrl + X to exit
Then I activated the configuration by linking to the config file from Nginx’s sites-enabled directory:

sudo ln -s /etc/nginx/sites-available/florintechlemp /etc/nginx/sites-enabled/

sudo nginx -t
To test the configuration for syntax errors
In order to disable default Nginx host that is currently configured to listen on port 80, I run

sudo unlink /etc/nginx/sites-enabled/default
Then reload Nginx to apply the changes:

sudo systemctl reload nginx
I created an index.html file in that location so that I can test that the new server block works as expected:

sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
Then I enter http://3.142.201.71:80 on my browser
![php2](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/d84d5574-0e76-4190-b7a1-8f8b9307a7e8)
### TESTING PHP WITH NGINX
In order to test to validate that Nginx can correctly hand .php files off to the PHP processor.

I created a test PHP file in the document root. This is by opening a new file called info.php within the document root in the text editor:
sudo nano /var/www/florintechlemp/info.php
Type or paste the following lines into the new file. This is valid PHP code that will return information about the server:
'<?php'
'phpinfo();'

Then I access the page in my web browser by visiting public IP address i have set up in the Nginx configuration file, followed by /info.php:
http://3.142.201.71/info.php



### RETRIEVING DATA FROM MYSQL DATABASE WITH PHP

![phpve](https://github.com/Oolabanji/DevOps_Projects/assets/136812420/0c39abe8-87a1-4fee-aab6-d22db04708d0)
### Retrieving data from MySQL database with PHP
First, I connected to the MySQL console using the root account:
$ sudo mysql
To create a new database, I run the following command from my MySQL console:

$ mysql> CREATE DATABASE `banji_database`;
Now I created a new user and granted it full privileges on the database I have just created.
$ mysql>  CREATE USER 'lemp_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
mysql> exit
I now  test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

mysql -u lemp_user -p
mysql> SHOW DATABASES;

![lempdb](https://github.com/Oolabanji/test_/assets/136812420/9f14fe72-1bd7-45e2-82fd-c22362ce8ef7)
Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:


I CREATE TABLE banji_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
mysql> INSERT INTO banji_database.todo_list (content) VALUES ("My first important item");
mysql> INSERT INTO banji_database.todo_list (content) VALUES ("My second important item");
mysql> INSERT INTO banji_database.todo_list (content) VALUES ("My third important item");
mysql> INSERT INTO banji_database.todo_list (content) VALUES ("My fourth important item");
To confirm that the data was successfully saved to the table, I run:

mysql>  SELECT * FROM lemp_database.todo_list;

![lempmysql](https://github.com/Oolabanji/test_/assets/136812420/57c2fa9d-33db-4298-b0ea-3fc0e4c08de6)

mysql> exit

I created a PHP script that will connect to MySQL and query for the content.

nano /var/www/florintechlemp/todo_list.php


I copy this content into the todo_list.php script:

'<?php'
$user = "example_user";
$password = "password";
$database = "banji_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

http://3.133.118.221/todo_list.php

![php4](https://github.com/Oolabanji/test_/assets/136812420/cfa2a8c0-c922-4e61-a6f1-aaac898af5e0)
