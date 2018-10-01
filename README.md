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
$ sudo apt-get update <br />
$ sudo apt-get upgrade <br />

# Change the SSH port from 22 to 2200
Use $ sudo vim /etc/ssh/sshd_config and then change Port 22 to Port 2200 , save & quit. <br />
Reload SSH using $ sudo service ssh restart <br />

# Configure the Uncomplicated Firewall (UFW) 
Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123) <br />

$ sudo ufw allow 2200/tcp <br />
$ sudo ufw allow 80/tcp <br />
$ sudo ufw allow 123/udp <br />
$ sudo ufw enable  <br />

# Configure the local timezone to UTC
Configure the time zone $ sudo dpkg-reconfigure tzdata <br />
Choose UTC. <br />

# Install and configure Apache to serve a Python mod_wsgi application
$ Install Apache sudo apt-get install apache2 <br />
$ Install mod_wsgi sudo apt-get install python-setuptools libapache2-mod-wsgi <br />
$ Restart Apache sudo service apache2 restart <br />

# Install and configure PostgreSQL
Install PostgreSQL $ sudo apt-get install postgresql <br />

Check if no remote connections are allowed $ sudo vim /etc/postgresql/9.3/main/pg_hba.conf <br />

Login as user "postgres" $ sudo su - postgres <br />

Get into postgreSQL shell  $ psql <br />

Create a new database named catalog and create a new user named catalog in postgreSQL shell <br />

postgres=# CREATE DATABASE catalog; <br />
postgres=# CREATE USER catalog; <br />
Set a password for user catalog <br />

postgres=# ALTER ROLE catalog WITH PASSWORD 'password'; <br />
Give user "catalog" permission to "catalog" application database <br />

postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog; <br />
Quit postgreSQL postgres=# \q <br />

Exit from user "postgres" <br />

exit <br />

# Install git, clone and setup your Catalog App project.
Install Git using $ sudo apt-get install git <br /> 
Use $ cd /var/www     to move to the /var/www directory <br />
Create the application directory $ sudo mkdir FlaskApp <br />
Move inside this directory using $ cd FlaskApp <br />
Clone the Catalog App to the virtual machine $ git clone https://github.com/mehulparmariitr/ItemCatalogUdacity <br />
Move all the files inside ItemCatalogUdacity\vagrant\catalog to a new folder FLaskApp <br />
Move to the inner FlaskApp directory using $ cd FlaskApp <br />
Rename application.py to __init__.py using $ sudo mv website.py __init__.py <br />
Edit database_setup.py, __init__.py and change *engine = create_engine('sqlite:///toyshop.db')* to *engine = create_engine('postgresql://catalog:password@localhost/catalog')* <br />
Install pip $ sudo apt-get install python-pip <br />
Install psycopg2 $ sudo apt-get -qqy install postgresql python-psycopg2 <br />
Create database schema $ sudo python database_setup.py <br />



