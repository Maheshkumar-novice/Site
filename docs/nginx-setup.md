# Nginx Setup Guide

Quick guide to deploy your static site on Ubuntu with Nginx.

---

## Prerequisites

- Ubuntu server with SSH access
- Domain DNS pointing to your server IP
- Sudo privileges

---

## 1. Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```

Verify it's running:
```bash
sudo systemctl status nginx
```

---

## 2. Create Site Directory

```bash
# Create directory
sudo mkdir -p /var/www/maheshkumar.blog

# Set ownership
sudo chown -R $USER:$USER /var/www/maheshkumar.blog
sudo chmod -R 755 /var/www/maheshkumar.blog
```

---

## 3. Copy Site Files

From your local machine:
```bash
# Using rsync
rsync -avz site/ user@your-server-ip:/var/www/maheshkumar.blog/

# Or using scp
scp -r site/* user@your-server-ip:/var/www/maheshkumar.blog/
```

Or if you're already on the server with the repo:
```bash
cp -r site/* /var/www/maheshkumar.blog/
```

---

## 4. Deploy Nginx Configuration

**Important**: If you already ran Certbot and have HTTPS working on the default site, skip to Option B.

### Option A: Fresh Setup (No HTTPS Yet)

Create a basic HTTP-only config first:

```bash
# Create temporary HTTP-only config
sudo tee /etc/nginx/sites-available/maheshkumar.blog > /dev/null <<'EOF'
server {
    listen 80;
    listen [::]:80;
    server_name maheshkumar.blog www.maheshkumar.blog;

    root /var/www/maheshkumar.blog;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

# Enable the site
sudo ln -s /etc/nginx/sites-available/maheshkumar.blog /etc/nginx/sites-enabled/

# Test and reload
sudo nginx -t
sudo systemctl reload nginx
```

Now proceed to the [HTTPS Setup Guide](https-setup.md) to run Certbot.

### Option B: After Certbot/Already Have HTTPS

If Certbot already created certificates for your domain:

```bash
# Copy the full config from this repo
sudo cp server/nginx.conf /etc/nginx/sites-available/maheshkumar.blog

# Enable your site
sudo ln -s /etc/nginx/sites-available/maheshkumar.blog /etc/nginx/sites-enabled/

# Disable default site (optional but recommended)
sudo rm /etc/nginx/sites-enabled/default

# Test and reload
sudo nginx -t
sudo systemctl reload nginx
```

---

## 5. Verify

Visit your site:
```bash
# HTTP (will redirect to HTTPS if configured)
curl -I http://maheshkumar.blog

# HTTPS
curl -I https://maheshkumar.blog
```

Open in browser: `https://maheshkumar.blog`

---

## Updating Site Content

To update your site later:

```bash
# Edit files locally, then rsync to server
rsync -avz site/ user@your-server-ip:/var/www/maheshkumar.blog/
```

No nginx reload needed for content changes.

---

## Common Issues

**"nginx: [emerg] bind() to 0.0.0.0:80 failed"**
- Another service is using port 80. Check with: `sudo lsof -i :80`

**"403 Forbidden" error**
- Fix permissions: `sudo chmod -R 755 /var/www/maheshkumar.blog`

**"502 Bad Gateway" or site not loading**
- Check error log: `sudo tail -f /var/log/nginx/error.log`
- Verify nginx is running: `sudo systemctl status nginx`

**SSL certificate errors after deploying full config**
- Make sure Certbot ran for your domain first
- Check certificates exist: `sudo ls -la /etc/letsencrypt/live/maheshkumar.blog/`

---

## Next Steps

If you haven't set up HTTPS yet, see the [HTTPS Setup Guide](https-setup.md).
