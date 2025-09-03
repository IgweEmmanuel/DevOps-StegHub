# WEB STACK IMPLEMENTATION (LEMP STACK)

## Step1: Installing nginx web server

```bash
sudo apt update
sudo apt install nginx
```

![nginx installation](./assets/nginx-installation.png 'Nginx Installation')

#### How to access our server through port 80

```bash
curl http://localhost:80
#OR
curl http://127.0.0.1:80
```

#### How to check for your public ip address

```bash
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4

#OR
curl ifconfig.me
```

![Nginx Output](./assets/nginx-output.png 'Nginx Output')

## Step 2: Installing MySQL

- Install mysql-server

```bash
sudo apt install mysql-server
```

_MySQL Installation_
![mysql installation](./assets/mysql-installation.png 'MySQL Installtion')

- Start the mysql using:

```bash
sudo mysql
```

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

### MySQL Running on the Terminal

![mysql running](./assets/mysql-running.png 'Running MySQL')

## Step 3 – Installing PHP

```bash
sudo apt install php-fpm php-mysql
```

![Php installation](./assets/php-installation.png 'PHP Installation')

## Step 4 — Configuring Nginx to Use PHP Processor

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

- Create the root web directory for your_domain as follows:

```bash
sudo mkdir /var/www/igweLEMP
```

- Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

```bash
sudo chown -R $USER:$USER /var/www/igweLEMP
```

- Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use vim:

```bash
sudo vim /etc/nginx/sites-available/igweLEMP
```

- Paste in the following bare-bones configuration:

```bash
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name igweLEMP www.igweLEMP;
    root /var/www/igweLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

- Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```

This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:

```bash
sudo nginx -t
```

_Here is the result of the configuration test_

![Configuration file](./assets//configuration.png 'configuration')

-We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

```bash
sudo unlink /etc/nginx/sites-enabled/default
```

- Reload Nginx to apply the changes:

```bash
sudo systemctl reload nginx
```

- To get our public ip address type:

```bash
curl ifconfig.me
```

- Now go to your browser and try to open your website URL using IP address:

```bash
http://13.48.203.235:80
```

- Our website is now active, but the web root /var/www/igweLEMP is still empty. Create an index.html file in that location so that we can test that our new server block works as expected:

![browser result](./assets/browser_nginx_result.png 'Browser Result')
