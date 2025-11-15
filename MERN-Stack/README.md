# MERN Stack Todo Application - AWS EC2 Deployment

## Overview

Full-stack MERN (MongoDB, Express, React, Node.js) Todo application deployed on AWS EC2 with MongoDB Atlas.

## Technology Stack

- **Backend**: Node.js + Express.js (v5.1.0)
- **Frontend**: React (v19.2.0) with Axios
- **Database**: MongoDB Atlas (Cloud)
- **Deployment**: AWS EC2 (Ubuntu)

## Application Structure

```
Todo/
├── index.js                    # Express server entry point
├── package.json                # Backend dependencies
├── .env                        # MongoDB connection string
├── models/
│   └── todo.js                # Mongoose Todo schema
├── routes/
│   └── api.js                 # API endpoints (GET, POST, DELETE)
└── client/                    # React frontend
    ├── package.json           # Frontend dependencies
    ├── .env                   # React environment config
    ├── public/                # Static assets
    └── src/
        ├── App.js             # Main React component
        ├── components/
        │   ├── Todo.js        # Main Todo container
        │   ├── Input.js       # Add todo input component
        │   └── ListTodo.js    # Todo list display component
        └── index.js           # React entry point
```

## API Endpoints

- **GET /api/todos** - Fetch all todos
- **POST /api/todos** - Create new todo (requires `action` field)
- **DELETE /api/todos/:id** - Delete todo by ID

## Frontend Features

- Add new todos via input field
- Display all todos in a list
- Delete todos by clicking on them
- Real-time updates after add/delete operations
- Proxy configuration to backend API (port 5000)

## Quick Setup

### Prerequisites

- AWS EC2 instance (Ubuntu)
- Node.js 18+ and npm
- MongoDB Atlas account

### Backend Setup

1. **Install dependencies:**

```bash
cd Todo
npm install
```

2. **Configure environment variables (.env):**

```env
DB=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/?appName=<AppName>
PORT=5000
```

3. **Start backend server:**

```bash
npm start                # Production
npm run start-watch      # Development with nodemon
```

### Frontend Setup

1. **Install dependencies:**

```bash
cd Todo/client
npm install
```

2. **Configure environment variables (client/.env):**

```env
HOST=0.0.0.0
DANGEROUSLY_DISABLE_HOST_CHECK=true
```

3. **Start React app:**

```bash
npm start                # Runs on port 3000
```

### Full-Stack Development

Run both backend and frontend concurrently:

```bash
cd Todo
npm run dev
```

## AWS EC2 Deployment

### 1. EC2 Instance Setup

**Launch Instance:**

- AMI: Ubuntu Server 20.04 LTS
- Instance Type: t2.micro (free tier)
- Storage: 8GB minimum

**Security Group Rules:**

- Port 22 (SSH): Your IP
- Port 5000 (Backend API): 0.0.0.0/0
- Port 3000 (React Dev): 0.0.0.0/0
- Port 80 (HTTP): 0.0.0.0/0 (production)

### 2. Server Configuration

**Connect to EC2:**

```bash
ssh -i your-key.pem ubuntu@<ec2-public-ip>
```

**Install Node.js:**

```bash
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
node --version && npm --version
```

### 3. Deploy Application

**Upload files to EC2 and install dependencies:**

```bash
cd Todo
npm install
cd client
npm install
cd ..
```

**Build React for production:**

```bash
cd client
npm run build
cd ..
```

### 4. Run with PM2 (Production)

**Install PM2:**

```bash
sudo npm install -g pm2
```

**Start backend:**

```bash
pm2 start index.js --name "todo-backend"
```

**Serve React build (optional - use Nginx instead):**

```bash
cd client
pm2 serve build 3000 --name "todo-frontend" --spa
```

**Save PM2 configuration:**

```bash
pm2 save
pm2 startup
```

## MongoDB Atlas Configuration

1. Create cluster at https://www.mongodb.com/cloud/atlas
2. Create database user with password
3. Whitelist EC2 IP or allow all (0.0.0.0/0)
4. Copy connection string to `.env` file

## Production Deployment with Nginx

**Install Nginx:**

```bash
sudo apt install nginx -y
```

**Configure Nginx (/etc/nginx/sites-available/default):**

```nginx
server {
    listen 80;
    server_name <your-domain-or-ip>;

    # Serve React build
    location / {
        root /home/ubuntu/Todo/client/build;
        try_files $uri /index.html;
    }

    # Proxy API requests to backend
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Restart Nginx:**

```bash
sudo nginx -t
sudo systemctl restart nginx
```

## Testing the Application

**Test Backend API:**

```bash
# Get all todos
curl http://<ec2-ip>:5000/api/todos

# Create todo
curl -X POST http://<ec2-ip>:5000/api/todos \
  -H "Content-Type: application/json" \
  -d '{"action": "Test todo"}'

# Delete todo
curl -X DELETE http://<ec2-ip>:5000/api/todos/<todo-id>
```

**Access Frontend:**

- Development: http://<ec2-ip>:3000
- Production (Nginx): http://<ec2-ip>

## PM2 Commands

```bash
pm2 status              # Check status
pm2 logs                # View logs
pm2 restart all         # Restart all apps
pm2 stop all            # Stop all apps
pm2 delete all          # Remove all apps
```

## Troubleshooting

**Backend won't start:**

- Check MongoDB connection string in `.env`
- Verify MongoDB Atlas IP whitelist
- Check port 5000 is not in use: `lsof -i :5000`

**Frontend can't connect to backend:**

- Verify proxy setting in `client/package.json`: `"proxy": "http://localhost:5000"`
- Check backend is running on port 5000
- Verify CORS is enabled in backend

**EC2 connection issues:**

- Check security group allows required ports
- Verify EC2 instance is running
- Check firewall: `sudo ufw status`

## Security Best Practices

1. **Never commit `.env` files** - Use AWS Secrets Manager
2. **Restrict MongoDB Atlas IP whitelist** to EC2 IP only
3. **Use strong passwords** for database
4. **Disable CORS wildcard** in production - specify allowed origins
5. **Enable HTTPS** with Let's Encrypt SSL certificates
6. **Keep packages updated** - Run `npm audit fix`
7. **Use environment-specific configs** for dev/prod

## Project Information

**Author:** Igwe Emmanuel  
**Version:** 1.0.0  
**License:** ISC

## Key Dependencies

**Backend:**

- express: ^5.1.0
- mongoose: ^8.18.1
- dotenv: ^17.2.2
- nodemon: ^3.1.11 (dev)

**Frontend:**

- react: ^19.2.0
- axios: ^1.13.2
- react-scripts: 5.0.1

## Resources

- [MongoDB Atlas](https://docs.atlas.mongodb.com/)
- [Express.js](https://expressjs.com/)
- [React](https://react.dev/)
- [AWS EC2](https://docs.aws.amazon.com/ec2/)
- [PM2](https://pm2.keymetrics.io/)
- [Nginx](https://nginx.org/en/docs/)
