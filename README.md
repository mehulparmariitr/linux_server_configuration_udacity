# linux_server_configuration_udacity
In this project, a Linux virtual machine needs to be configurated to support the Item Catalog website.

Local IP address : http://54.236.95.0/
The EC2 URL is : http://ec2-54-236-95-0.compute-1.amazonaws.com/
SSH port - 2200
Private key - In Note's to reviewer field
Public Key - In Note's to reviewer field
Login with: ssh -i ~/.ssh/id_rsa -p 2200 grader@54.236.95.0

# STEPS FOLLOWED

# Create a new user named grader
sudo adduser grader <br />
vim /etc/sudoers <br />
touch /etc/sudoers.d/grader <br />
vim /etc/sudoers.d/grader, type in grader ALL=(ALL:ALL) ALL, save and quit <br />

# Set ssh login using keys
generate keys on local machine using $ ssh-keygen <br />
private key was made by name ~/.ssh/id_rsa <br />
public key was  ~/.ssh/id_rsa.pub <br />

On you virtual machine: <br />

$ su - grader <br />
$ mkdir .ssh <br />
$ touch .ssh/authorized_keys <br />
$ vim .ssh/authorized_keys <br />

Copied the public key(*id_rsa.pub*) generated on local machine to this file(authorized_keys) and save <br />

Then copy *authorized keys* to grader ssh folder also <br />
$ cp /root/.ssh/authorized_keys /home/grader/.ssh/authorized_key

Change permissions <br />
$ chmod 700 .ssh <br />
$ chmod 644 .ssh/authorized_keys <br />


reload SSH using $ service ssh restart <br />

now we can use ssh to login with the new user created <br />
$ ssh -i ~/.ssh/id_rsa -p 2200 grader@54.236.95.0 <br />

# Update all installed packages
$ sudo apt-get update
$ sudo apt-get upgrade

# Change the SSH port from 22 to 2200
Use $ sudo vim /etc/ssh/sshd_config and then change Port 22 to Port 2200 , save & quit.
Reload SSH using $ sudo service ssh restart

# Configure the Uncomplicated Firewall (UFW)
Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

$ sudo ufw allow 2200/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 123/udp
$ sudo ufw enable 

# Configure the local timezone to UTC
Configure the time zone $ sudo dpkg-reconfigure tzdata
It is already set to UTC.

# Install and configure Apache to serve a Python mod_wsgi application
Install Apache sudo apt-get install apache2
Install mod_wsgi sudo apt-get install python-setuptools libapache2-mod-wsgi
Restart Apache sudo service apache2 restart




