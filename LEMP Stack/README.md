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
