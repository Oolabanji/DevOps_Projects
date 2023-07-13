## Deploying a MEAN Stack Application on AWS Cloud
I  created an aws EC2 instance, named MERN. This will serve as the backbone of the application deployment.
![MERNEC2](https://github.com/Oolabanji/test_/assets/136812420/e7bd8a10-6e41-48d9-b0e7-559a48910d71)
Then I  update and upgrade core dependencies on the linux backbone

![aptupdate](https://github.com/Oolabanji/test_/assets/136812420/51ec7f95-9b2a-4df0-8266-e88dbea9dd4e)
![aptupgrade](https://github.com/Oolabanji/test_/assets/136812420/ac58351c-eef2-44ff-a7f9-73461ccf945b)
### Applying certificates and installing nodejs
'sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates'

'curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -'

![nodeinstallation](https://github.com/Oolabanji/test_/assets/136812420/8d197b59-a2e8-4857-9b7d-e8140dbe90e4)
We then proceed to install mongodb which is a non-relational database which we will use to store our applications data.

'sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6'
'echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list'
'sudo apt install -y mongodb'

'sudo systemctl start mongodb'
'sudo systemctl enable mongodb'
'sudo systemctl status mongodb'
![mongodbstatus](https://github.com/Oolabanji/test_/assets/136812420/b9beab36-289b-4140-92dd-f1ee3d167b3d)
I Installed npm which is the default package manager for JavaScript's runtime Node.js.
![npm](https://github.com/Oolabanji/test_/assets/136812420/c345ce32-c35a-42ab-b410-ea718c4b8ae9)
![bodyparser](https://github.com/Oolabanji/test_/assets/136812420/509c59dd-86cd-4c7c-a944-271e442fdecf)
I created a Books directory and initialize it as a npm project using npm init. Then create a server.js file and setup the server.
'nano server.js'
![serverjs](https://github.com/Oolabanji/test_/assets/136812420/9f99fcd9-c4a7-456c-9932-852eae1d194b)
I Installed express and mongoose which is a package which provides a straight-forward, schema-based solution to model my application data. I used Mongoose to establish a schema for the database to store data of the book register.
![expressmongoose](https://github.com/Oolabanji/test_/assets/136812420/bbce8725-e30b-4869-af80-d69a20e2920c)
In the books directory, I created a directory apps and created a routes.js file then append the code to it.

![routesjs](https://github.com/Oolabanji/test_/assets/136812420/2bfe5d34-ebc5-434f-bf05-520a93b6a5e9)
I created a directory models in the books directory and add a file books.js and append the code which contains the schema model
![bookjs](https://github.com/Oolabanji/test_/assets/136812420/c04bd489-7dd4-414b-bd32-8219718ca904)
In the book directory I created a public directory and created a script.js file which will contain our angular frontend code.

![scriptjs](https://github.com/Oolabanji/test_/assets/136812420/3aad2337-99f3-444f-888c-d703244947a1)
I created an index.html in the public directory and append the code
![indexhtml](https://github.com/Oolabanji/test_/assets/136812420/5fede62e-d27d-4a86-b04a-935be3e79abf)
I move into the books directory and spin up the express server using node server.js
![nodeserverjs](https://github.com/Oolabanji/test_/assets/136812420/898120d7-64c7-4d82-96cd-168ee748c503)
I configured security group inbound rules to allow the application to be accessible via the internet via our server port

![adjustsecuritygroup](https://github.com/Oolabanji/test_/assets/136812420/033f5d8f-b9b2-452c-9657-b34951e18708)
On a browser, I paste the public ip address of the instance to view the site

![BookDirectoryFinal](https://github.com/Oolabanji/test_/assets/136812420/d4194209-1d81-4252-9942-6841594e631f)