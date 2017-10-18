# Project 5: Linux Server Configuration Project
### by Jaime Banda

This is the fifth project of the [Udacity Full-Stack Nanodegree - Linux Server Configuration,
Nanodegree](https://classroom.udacity.com/nanodegrees/nd004/parts/8d3e23e1-9ab6-47eb-b4f3-d5dc7ef27bf0/modules/348776022975462/lessons/3487760229239847/concepts/36269487530923).

## What it is and does

This is the baseline installation of a Linux server and prepare it to host a Flask web application. The server will be secure from a number of attack vectors, by installing and configuring a database server, a firewall, updates and deploy an existing web application onto it.

Here are some URLs structures that the application follow to manage the catalog:


## Get a server on Lightsail 


We're going to get a server in Amazon Lightsail for this project. There are a few things you need to do when you create your server instance.

1. Log in!
First, log in to Lightsail. If you don't already have an Amazon Web Services account, you'll be prompted to create one. Amazon Web Services login page.

2. Create an instance.
Once you're logged in, Lightsail will give you a friendly message with a robot on it, prompting you to create an instance. A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter. When you have no instances, Lightsail gives you a picture of an orange robot and suggests that you create an instance.

3. Choose an instance image: Ubuntu
Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.

For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.When you create an instance, Lightsail asks what kind you want.
For this project, choose an "OS Only" instance with Ubuntu.

4. Choose your instance plan.
The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero. Lightsail's options for instance pricing.
For this project, pick the lowest one to get free-tier access.
Be aware: If you enable additional features in Lightsail, you may be charged extra for them.

5. Give your instance a hostname.
Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.

6. Wait for it to start up.
It may take a few minutes for your instance to start up. While your instance is starting up, Lightsail shows you a grayed-out display.Once your instance is running, the display gets brighter.

7. It's running; let's use it!
Once your instance has started up, you can log into it with SSH from your browser. The public IP address of the instance is displayed along with its name. In the above picture it's 35.165.26.190.

8. Project time.
Now that you have a working instance, you can get right into the project!


## Setting up the server

1. Update the available package list
```
sudo apt-get update
```

2. Upgrade installed packages.
```
sudo apt-get upgrade
```

3. Remove packages no longer required (automatically removed).
```
sudo apt-get autoremove
```
4. Install finger to get information about a user.
```
sudo apt-get install finger
```

5. Upgrade python2  python3 & memcached
```
sudo apt-get upgrade python2
sudo apt-get upgrade python3
sudo apt-get install memcached
```

7. Install Apache httpd server
```
sudo apt-get install apache2
```

8. Create a new user for grader
```
sudo adduser grader
```

9. Add user to the sudoers by creating the file with these commands
```
sudo cp /etc/sudoers.d/90-init-user /etc/sudoers.d/grader
sudo nano /etc/sudoers.d/grader
```

10. Change the user from '90-init-user' to 'grader'
```
grader ALL=(ALL) NOPASSWD:ALL
```

11. Generate key-pair for user grader (NOT ON THE SERVER, GENERATE IT IN YOUR PC)
```
ssh-keygen
```

12. Create and change ownership of the files
```
sudo mkdir /home/grader/.ssh
sudo touch /home/grader/.ssh/authorized_keys
sudo chown grader /home/grader/.ssh/authorized_keys
sudo chown :grader /home/grader/.ssh/authorized_keys
sudo chown grader /home/grader/.ssh
sudo chown :grader /home/grader/.ssh
```

13. Copy the content of the key in the server to the `/home/gader/.ssh/authorized_keys` file

14. Change the permission on the .ssh directory and on the authorized_key file.
```
sudo chmod 700 /home/grader/.ssh
sudo chmod 644 /home/grader/.ssh/.authorized_keys
```

15. Forcing key based authentication and update ssh port from 22 to 2200
```
sudo nano /etc/ssh/sshd_config
```

update file on:
```
.
.
Port 22000
.
.
PasswordAuthentication no

```

16. Open port 2200 on the network tab on the Lightsail website

17. Install git with `sudo apt-get install git`

18. If project built in python 3, install the python3 mod_wsgi package on the server.
```
sudo apt-get install libapache2-mod-wsgi-py3
```

19. Install pip for python2 and python3
```
sudo apt-get install pip
sudo apt-get install pip3
```

20. Restar the ssh service
```
sudo service ssh restard
```

21. Setting up the firewall
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200/tcp
sudo ufw allow ssh
sudo ufw allow www
sudo ufw allow ntp
sufo ufw allow 123/udp
sudo ufw enable
```

22. Configure apache to hand-off certain requests to an application handler-mod-wsgi. The first step is to install mod_wsgi
```
sudo apt-get install libapache2-mod_wsgi
```

## Install PostgreSQL

1. `sudo apt-get install libpq-dev python-dev`
2. `sudo apt-get install postgresql postgresql-contrib`
3. `sudo su - postgres`
4. `psql`
5. `CREATE USER catalog WITH PASSWORD 'password';`
6. `ALTER USER catalog CREATEDB;`
7. `CREATE DATABASE catalog WITH OWNER catalog;`
8. `\c catalog`
9. `REVOKE ALL ON SCHEMA public FROM public;`
10. `GRANT ALL ON SCHEMA public TO catalog;`
11. `\q`
12. `exit`
13. `Change create engine line in your __init__.py and db.py to: engine = create_engine('postgresql://catalog:password@localhost/catalog')`

## Project contents

This project consists for the following files:

* readme.md. - Steps to setup the server.
* graderkey - SSH private key to access the server 35.165.26.190.

## How to Run Project

Connect to the server from terminal with the following instruction

```
ssh grader@35.165.26.190 -p 2200 -i ~/.ssh/graderkey
```

### Access the current project website
Open your browser to the following URL:
```
http://35.165.26.190/
```
