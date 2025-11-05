# Deployment Guide - Italian Checkout Page

## Deploying to Your VPS

### Prerequisites
- Node.js v18 or higher
- npm or pnpm
- A VPS with Ubuntu/Debian or similar Linux distribution

### Step 1: Upload Project Files

Upload all project files to your VPS (using SFTP, SCP, or Git).

```bash
# Example using SCP from your local machine
scp -r /path/to/project user@your-vps-ip:/var/www/checkout-app
```

### Step 2: Install Dependencies

```bash
cd /var/www/checkout-app
npm install
```

### Step 3: Set Environment Variables

Create a `.env` file or set environment variables:

```bash
export SESSION_SECRET="generate-a-random-secure-string-here"
export NODE_ENV="production"
```

### Step 4: Build the Frontend

```bash
npm run build
```

### Step 5: Start the Application

**For testing:**
```bash
npm start
```

**For production (using PM2):**
```bash
# Install PM2 globally
npm install -g pm2

# Start the application
pm2 start npm --name "checkout-app" -- start

# Save PM2 configuration
pm2 save

# Setup PM2 to start on system boot
pm2 startup
```

### Step 6: Configure Nginx (Recommended)

Install Nginx:
```bash
sudo apt update
sudo apt install nginx
```

Create Nginx configuration (`/etc/nginx/sites-available/checkout-app`):

```nginx
server {
    listen 80;
    server_name yourdomain.com;  # Replace with your domain or IP

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable the site:
```bash
sudo ln -s /etc/nginx/sites-available/checkout-app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### Step 7: Setup SSL (Optional but Recommended)

Using Let's Encrypt:
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com
```

### Application Structure

- `client/` - React frontend application
- `server/` - Express backend server
- `shared/` - Shared types and schemas
- `attached_assets/` - Logo images (Poste Italiane, BRT)

### Important Notes

1. **Port**: The application runs on port 5000 by default
2. **Database**: Uses in-memory storage (data will be lost on restart)
3. **Session Secret**: Must be set for security
4. **Firewall**: Make sure port 80/443 is open on your VPS

### Troubleshooting

**Application won't start:**
- Check Node.js version: `node --version`
- Check logs: `pm2 logs checkout-app`

**Can't connect from browser:**
- Check firewall: `sudo ufw status`
- Check if app is running: `pm2 status`
- Check Nginx: `sudo systemctl status nginx`

**Orders not saving:**
- This app uses in-memory storage. Orders will be lost on restart.
- For persistent storage, you would need to integrate a database.

### Monitoring

View application logs:
```bash
pm2 logs checkout-app
```

Check application status:
```bash
pm2 status
```

Restart application:
```bash
pm2 restart checkout-app
```
