# Nginx Setup Guide for maheshkumar.blog

This guide provides step-by-step instructions for setting up Nginx on an Ubuntu DigitalOcean droplet to serve your static portfolio website.

---

## Overview

This guide covers:

- Installing Nginx on Ubuntu
- Understanding Nginx directory structure
- Creating directories for site files
- Deploying Nginx configuration from this repository
- Testing and activating your site
- Integration with Certbot for HTTPS

Follow these steps in order for a successful deployment.

---

## Prerequisites

Before starting, ensure you have:

1. **Ubuntu Server**: Fresh DigitalOcean droplet or similar VPS running Ubuntu 20.04 LTS or later
2. **SSH Access**: Ability to connect to your server via SSH
3. **Sudo Privileges**: User account with sudo permissions
4. **Domain Configured**: DNS records pointing to your server IP (see HTTPS setup guide)
5. **Repository Access**: This repository cloned or files downloaded to your local machine

---

## Installing Nginx

Nginx is available in Ubuntu's default package repository.

### Update Package Lists

First, ensure your package lists are up to date:

```bash
sudo apt update
```

This command fetches the latest package information from Ubuntu's repositories.

### Install Nginx

Install Nginx with the following command:

```bash
sudo apt install nginx -y
```

The `-y` flag automatically confirms installation prompts.

### Enable Nginx to Start on Boot

Configure Nginx to start automatically when the server boots:

```bash
sudo systemctl enable nginx
```

### Start Nginx Service

Start the Nginx service:

```bash
sudo systemctl start nginx
```

### Verify Installation

Check that Nginx is running:

```bash
sudo systemctl status nginx
```

You should see output indicating the service is "active (running)".

### Verify Web Server Access

Check Nginx is responding to requests:

```bash
curl http://localhost
```

You should see HTML output from the default Nginx welcome page.

Alternatively, visit your server's IP address in a browser: `http://xxx.yyy.zzz.aaa` (replace with your actual IP).

---

## Understanding Nginx Directory Structure

Familiarity with Nginx's directory structure helps you manage configurations effectively.

### Key Directories

| Directory | Purpose |
|-----------|---------|
| `/etc/nginx/` | Main configuration directory |
| `/etc/nginx/nginx.conf` | Master configuration file |
| `/etc/nginx/sites-available/` | Configuration files for all sites |
| `/etc/nginx/sites-enabled/` | Symbolic links to active site configurations |
| `/var/www/` | Default location for website files |
| `/var/log/nginx/` | Access and error logs |

### How Sites Work

1. **sites-available**: Contains configuration files for all sites (active and inactive)
2. **sites-enabled**: Contains symbolic links to configurations in `sites-available` for active sites
3. Nginx only loads configurations from `sites-enabled`

This structure allows you to maintain multiple site configurations and easily enable/disable them without deleting files.

---

## Creating the Site Directory

Create a directory structure to store your website files.

### Create Directory

```bash
sudo mkdir -p /var/www/maheshkumar.blog
```

The `-p` flag creates parent directories if they don't exist.

### Set Ownership

Give ownership to your user account (replace `$USER` with your username if needed):

```bash
sudo chown -R $USER:$USER /var/www/maheshkumar.blog
```

This allows you to copy files without using sudo for each operation.

### Set Permissions

Set appropriate permissions:

```bash
sudo chmod -R 755 /var/www/maheshkumar.blog
```

- **755**: Owner can read/write/execute, group and others can read/execute
- This ensures Nginx can read files while maintaining security

### Verify Directory

Confirm the directory was created:

```bash
ls -la /var/www/
```

You should see `maheshkumar.blog` listed with your user as the owner.

---

## Preparing the Configuration File

The Nginx configuration for this site is located in the `server/` folder of this repository.

### Configuration File Location

In this repository: `server/nginx.conf`

### Understanding Placeholders

The configuration file uses placeholders for portability and security:

| Placeholder | What to Replace With |
|------------|---------------------|
| `/var/www/maheshkumar.blog` | Keep as-is, or use your preferred path |
| Certificate paths | These will be filled automatically by Certbot |

For initial setup (before running Certbot), you may want to temporarily comment out the HTTPS sections or use the configuration without SSL first.

### Initial Setup: HTTP Only (Optional)

If you want to test Nginx before setting up HTTPS, you can create a simplified HTTP-only configuration first, then run Certbot which will add HTTPS sections automatically.

---

## Deploying the Configuration

Copy the configuration file from this repository to your server.

### Copy Configuration to sites-available

From your local machine, transfer the nginx.conf to your server (replace `your_server_ip` with actual IP):

```bash
scp server/nginx.conf your_user@xxx.yyy.zzz.aaa:/tmp/
```

Then on the server, move it to the correct location:

```bash
sudo cp /tmp/nginx.conf /etc/nginx/sites-available/maheshkumar.blog
```

Alternatively, if you're already on the server with the repository cloned:

```bash
sudo cp server/nginx.conf /etc/nginx/sites-available/maheshkumar.blog
```

### Enable the Site

Create a symbolic link from `sites-enabled` to `sites-available`:

```bash
sudo ln -s /etc/nginx/sites-available/maheshkumar.blog /etc/nginx/sites-enabled/
```

### Remove Default Site (Optional)

If this is the only site on your server, remove the default Nginx site:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

This prevents conflicts and ensures your site is served correctly.

---

## Testing and Activating

Before reloading Nginx, always test the configuration for syntax errors.

### Test Nginx Configuration

```bash
sudo nginx -t
```

**Expected output** for valid configuration:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

**If you see errors**:
- Check file paths in the configuration
- Ensure certificate files exist (if HTTPS is configured)
- Review the error message for specific issues
- For HTTPS errors before Certbot setup, see note below

### Initial Setup: Handle HTTPS Sections

If you haven't run Certbot yet and your configuration includes HTTPS sections, you have two options:

**Option 1**: Comment out HTTPS server blocks temporarily:
- Edit `/etc/nginx/sites-available/maheshkumar.blog`
- Comment out lines 14-74 (the HTTPS server blocks)
- Test and reload Nginx
- Run Certbot (it will modify the config)

**Option 2**: Use Certbot immediately (recommended):
- Follow the HTTPS setup guide first
- Certbot will validate and configure HTTPS
- Then proceed with testing

### Reload Nginx

If the test passes, reload Nginx to apply the configuration:

```bash
sudo systemctl reload nginx
```

Alternatively, restart for a full restart:

```bash
sudo systemctl restart nginx
```

### Verify Service Status

Confirm Nginx reloaded successfully:

```bash
sudo systemctl status nginx
```

---

## Copying Site Files

Transfer your website files from the `site/` directory to the server.

### Copy Files from Local Machine

Using scp from your local machine:

```bash
scp -r site/* your_user@xxx.yyy.zzz.aaa:/var/www/maheshkumar.blog/
```

Or using rsync for better handling:

```bash
rsync -avz site/ your_user@xxx.yyy.zzz.aaa:/var/www/maheshkumar.blog/
```

### Verify Files

On the server, check files were copied correctly:

```bash
ls -la /var/www/maheshkumar.blog/
```

You should see:
- `index.html`
- `styles.css`
- `script.js`

### Set Permissions (if needed)

Ensure Nginx can read the files:

```bash
sudo chown -R www-data:www-data /var/www/maheshkumar.blog
sudo chmod -R 755 /var/www/maheshkumar.blog
```

### Test the Site

Visit your domain in a browser:
- HTTP: `http://maheshkumar.blog` (before HTTPS setup)
- HTTPS: `https://maheshkumar.blog` (after Certbot)

You should see the portfolio page with your name and three links.

---

## Integration with Certbot

After your site is working over HTTP, proceed with HTTPS setup.

### Order of Operations

1. ✅ Install Nginx (completed above)
2. ✅ Deploy basic HTTP configuration (completed above)
3. ✅ Copy site files (completed above)
4. ✅ Verify HTTP site works
5. ⏭️ **Next**: Run Certbot (see `docs/https-setup.md`)
6. ⏭️ Certbot will modify Nginx config for HTTPS
7. ⏭️ Verify HTTPS site works

### Reference HTTPS Setup Guide

For detailed Certbot setup instructions, see:

**[HTTPS Setup Guide](https-setup.md)**

The HTTPS guide covers:
- Installing Certbot
- Running certificate generation
- Automatic Nginx configuration by Certbot
- Certificate renewal setup

### What Certbot Will Do

When you run Certbot with the `--nginx` flag, it will:

1. Verify domain ownership
2. Generate SSL certificates
3. Automatically modify `/etc/nginx/sites-available/maheshkumar.blog`
4. Add SSL certificate paths
5. Configure HTTPS redirects
6. Reload Nginx

If you're using the full configuration from this repository (with HTTPS sections), Certbot may skip some modifications since they're already present.

---

## Maintenance and Updates

### Updating Site Files

To update your site content:

1. Edit files locally in the `site/` directory
2. Copy updated files to server:
   ```bash
   rsync -avz site/ your_user@xxx.yyy.zzz.aaa:/var/www/maheshkumar.blog/
   ```
3. No Nginx reload needed for content changes

### Updating Nginx Configuration

To update Nginx configuration:

1. Edit `server/nginx.conf` locally
2. Copy to server:
   ```bash
   scp server/nginx.conf your_user@xxx.yyy.zzz.aaa:/tmp/
   ```
3. On server:
   ```bash
   sudo cp /tmp/nginx.conf /etc/nginx/sites-available/maheshkumar.blog
   sudo nginx -t
   sudo systemctl reload nginx
   ```

### Viewing Logs

Monitor site access and errors:

```bash
# View access log
sudo tail -f /var/log/nginx/maheshkumar.blog.access.log

# View error log
sudo tail -f /var/log/nginx/maheshkumar.blog.error.log
```

---

## Troubleshooting

### Site Not Loading

**Check Nginx is running**:
```bash
sudo systemctl status nginx
```

**Check configuration syntax**:
```bash
sudo nginx -t
```

**Check error logs**:
```bash
sudo tail -n 50 /var/log/nginx/maheshkumar.blog.error.log
```

### Permission Denied Errors

**Fix file ownership**:
```bash
sudo chown -R www-data:www-data /var/www/maheshkumar.blog
sudo chmod -R 755 /var/www/maheshkumar.blog
```

### Configuration Changes Not Applied

**Reload Nginx**:
```bash
sudo systemctl reload nginx
```

**Or restart if reload doesn't work**:
```bash
sudo systemctl restart nginx
```

---

## Summary

You have now configured Nginx to serve your static portfolio website. Your setup includes:

- Nginx installed and running
- Site directory created at `/var/www/maheshkumar.blog`
- Configuration deployed to `/etc/nginx/sites-available/`
- Site enabled and accessible

**Next Steps**:
1. Verify your HTTP site works: `http://maheshkumar.blog`
2. Follow the [HTTPS Setup Guide](https-setup.md) to enable SSL/TLS
3. Test HTTPS site: `https://maheshkumar.blog`
4. Monitor logs and maintain your site

---
