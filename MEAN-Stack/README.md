# MEAN Stack Deployment to Ubuntu in AWS

---

## Install MongoDB

- Download mongodb

```sh
sudo apt-get install -y gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```

![Mongodb download](./assets/mongo-download-update.png 'Mongodb download')

- Make the package

```sh
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

- Install MongoDB

```sh
sudo apt-get install -y mongodb-org
```

![Mongodb install](./assets/mongodb-install.png 'Mongodb install')

- Start the server

```sh
sudo service mongod start
OR
sudo systemctl start mongod
```

![Mongodb-start](./assets/mongodb-start.png 'Mongodb-start')

- Verify the service is up and running

```sh
sudo systemctl status mongod
```

- Install npm

```sh
sudo apt install -y npm
```

- Install body-parser package (to help process JSON files passed in requests to the server)

```sh
sudo npm install body-parser
```

## Install Express and set up routes to the server

- Installing Express

```sh
sudo npm install express
```

- Installing Mongoose

```sh
sudo npm install mongoose
```

## Server connection

![Server Connection](./assets/server-connection.png 'Server Connection!')

## Output

![Book Management](./assets/bookmgmt.png 'Book Management')
