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
sudo adduser grader
vim /etc/sudoers
touch /etc/sudoers.d/grader
vim /etc/sudoers.d/grader, type in grader ALL=(ALL:ALL) ALL, save and quit

# Set ssh login using keys
generate keys on local machine using ssh-keygen ; 
private key was made by name ~/.ssh/id_rsa
public key was  ~/.ssh/id_rsa.pub

On you virtual machine:

$ su - grader
$ mkdir .ssh
$ touch .ssh/authorized_keys
$ vim .ssh/authorized_keys

Copied the public key(*id_rsa.pub*) generated on local machine to this file(authorized_keys) and save

$ chmod 700 .ssh
$ chmod 644 .ssh/authorized_keys
reload SSH using service ssh restart

now we can use ssh to login with the new user created
ssh -i ~/.ssh/id_rsa -p 2200 grader@54.236.95.0




