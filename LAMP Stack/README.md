# Installing Git and OpenSSH Server on WSL (Windows Subsystem for Linux)

This guide covers installing Git and OpenSSH Server on Windows Subsystem for Linux (WSL).

## Prerequisites

- Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11
- WSL installed and configured
- Ubuntu or your preferred Linux distribution running on WSL

## Installing Git on WSL

### Update Package Lists

First, ensure your package lists are up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

### install Git

```bash
sudo apt install git -y
```

### Verify Git installation

```bash
git --version
```

### Configure Git for first time setup

--

### Set your name and email

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Optional: Set default branch name

```bash
git config --global init.defaultBranch main
```

### Optional: Set default editor

```bash
git config --global core.editor "code --wait"  # For VS Code
# or
git config --global core.editor "nano"         # For nano
```

### Verify configuration

```bash
git config --list
```

## Installing OpenSSH Server on WSL

### Installing OpenSSH Server

```bash
sudo apt install openssh-server -y
```

### Configure SSH Server

```bash
sudo nano /etc/ssh/sshd_config
```

### Add or modify these lines in /etc/ssh/sshd_config:

```bash
# Change default port (optional, for security)
Port 2222

# Allow password authentication (can disable later after setting up keys)
PasswordAuthentication yes

# Allow public key authentication
PubkeyAuthentication yes

# Disable root login (recommended)
PermitRootLogin no

# Allow your user (replace 'username' with your actual username)
AllowUsers your_wsl_username
```

### Start and enable SSH Service

```bash
# Start SSH service
sudo service ssh start

# Check SSH service status
sudo service ssh status

# Optional: Auto-start SSH on WSL startup
echo 'sudo service ssh start' >> ~/.bashrc
```

# AWS Instance provisioning

Here are the concise steps to provision an AWS EC2 instance:

1. **Launch instance** - Go to EC2 console, click "Launch Instance"
2. **Choose AMI** - Select Amazon Machine Image (e.g., Amazon Linux 2023)
3. **Select instance type** - Pick size based on needs (t2.micro for free tier)
4. **Configure key pair** - Create new .pem key or select existing one
5. **Configure security group** - Set firewall rules (SSH port 22 at minimum)
6. **Review and launch** - Verify settings and click "Launch Instance"

The instance will be running within 1-2 minutes. Connect via SSH:

### Change ownership file access mode for our .pem key

```bash
chmod 400 igwe-ec2.pem
```

![EC2 instance](./assets/AWS-EC2-instance.png 'AWS Instance')

### Security Group - Inbound Rule

![Inbount Rule](./assets/inbound-rule.png 'Inbound Rule')

### Connecting to your EC2 Instance using SSH

```bash
ssh -i "igwe-ec2.pem" ubuntu@ec2-16-171-142-90.eu-north-1.compute.amazonaws.com
```

### Terminal Output

![ssh into server](./assets/ssh1.png 'Output 1')

![ssh into server](./assets/ssh2.png 'Output 2')

# Installing Apache and Updating the Firewall

### Install Apache2

```bash
#update a list of packages in package manager
$ sudo apt update
#run apache2 package installation
$ sudo apt install apache2
```

### Verify Apache is running as a service in our OS

```bash
$ sudo systemctl status apache2
```

![Running apache service](./assets/apache-running.png 'Apache running')

### Accessing our Apache Server locally in our ubuntu shell

```bash
$ curl http://localhost:80
or
$ curl http://127.0.0.1:80
```

### Apache server responds to request from web browser

- type in this to the address bar of any web browser

```bash
http://172.31.39.182:80
```

### The browser output

![Apache browser output](./assets/apache-browser-result.png 'Apache Browser Output')

# Installing MySQL

```bash
sudo apt install mysql-server
```

![MySQL Installation](./assets/mysql-installation.png 'MySQL Installation')

### Installing MySQL Secure Installation

- Set password for root user at the mysql already running:

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password1';
```

- Exit mysql
- Install mysql_secure_installation package and yes to all questions

```bash
sudo apt install mysql_secure_installation
```

![MySQL Secure Installation](./assets/mysql-secure-installation.png 'MySQL Secure Installation')
Now mysql is pretty much secured and prevents test user and test database for external or remote access.

# Installing PHP

We shall install PHP and its dependencies

```bash
sudo apt install php
```

```bash
# This installs every other dependencies required for PHP to interact with the Apache and MySQL
sudo apt install libapache2-mod-php
sudo apt install php-mysql
```

![PHP Installation](./assets/php-installation.png 'PHP Installation')

## Creating a Virtual Host for Website using Apache

- Add another Apache server block called igwe

```bash
sudo mkdir /var/www/igwe
```

- Assign ownership to the current user using the $USER environment variable

```bash
sudo chown -R $USER:$USER /var/www/igwe
```

- Open a new configuration file in apache sites-available directory

```bash
sudo vim /etc/apache2/sites-available/igwe.config
```

- Paste in this bare-bone as the igwe.config file is empty

```bash
<VirtualHost *:80>
    ServerName igwe
    ServerAlias www.igwe
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/igwe
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- Create an index.html file in igwe directory to display on the browser

_Apache2 Public IP browser Output_
![Apache2 Public IP Output](./assets/apache-ip-output.png 'Apache2 Public IP Output')

_Apache2 Public DNS browser Output_
![Apache2 DNS Output](./assets/apache-DNS-output.png 'Apache2 DNS Output')

## Enable PHP on the website

- Access the dir.conf file and change it

```bash
sudo vim /etc/apache2/mods-enabled/dir.conf
```

```bash
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

- reload apache2

```bash
$ sudo systemctl reload apache2
```

- Create a new file named index.php

```bash
$ sudo vim /var/www/igwe/index.php
```

- Use this code

```bash
<?php
  phpinfo();
?>
```

_PHP Info Output_

![PHP Info Output](./assets/phpinfo-output.png 'PHP Info Output')
