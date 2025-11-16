# HTTPS Setup Guide

Quick guide to enable HTTPS with Let's Encrypt and Certbot.

---

## Prerequisites

- Nginx installed and running
- Domain DNS records pointing to your server IP
- Ports 80 and 443 open in firewall

---

## DNS Configuration

Your domain must point to your server before Certbot can issue certificates.

Add these A records at your domain registrar:

| Type | Name | Value |
|------|------|-------|
| A | @ | your-server-ip |
| A | www | your-server-ip |

Verify DNS is working:
```bash
dig maheshkumar.blog +short
dig www.maheshkumar.blog +short
```

Both should return your server's IP address.

---

## Install Certbot

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

Verify installation:
```bash
certbot --version
```

---

## Run Certbot

### If You Have a Site Config Already

If you followed the nginx-setup guide and have a site config at `/etc/nginx/sites-enabled/maheshkumar.blog`:

```bash
sudo certbot --nginx -d maheshkumar.blog -d www.maheshkumar.blog
```

### If Using the Default Site

```bash
sudo certbot --nginx -d maheshkumar.blog -d www.maheshkumar.blog
```

### What Certbot Will Ask

1. **Email address**: Enter your real email (for renewal notifications)
   - Example: `your-email@example.com`

2. **Terms of Service**: Type `Y` to agree

3. **EFF Email List**: Type `Y` or `N` (your choice)

4. **Redirect HTTP to HTTPS**: Choose `2` (redirect) - **recommended**

### What Certbot Does

- Verifies you control the domain
- Generates SSL certificates
- Updates your Nginx config automatically
- Sets up auto-renewal

---

## Verify HTTPS Works

Test your site:
```bash
curl -I https://maheshkumar.blog
```

Open in browser: `https://maheshkumar.blog`

You should see the padlock icon in the address bar.

---

## Certificate Auto-Renewal

Certbot automatically sets up renewal. Certificates renew every 60 days.

### Test Renewal

```bash
sudo certbot renew --dry-run
```

If successful, you'll see: `Congratulations, all simulated renewals succeeded`

### Check Certificate Status

```bash
sudo certbot certificates
```

Shows certificate expiration dates.

---

## Common Issues

**"Failed authorization procedure" / "Connection refused"**
- DNS not pointing to your server yet. Wait for propagation.
- Port 80 is blocked. Check: `sudo ufw allow 80/tcp`

**"Certificate already exists"**
- Certificate was already generated. To regenerate:
  ```bash
  sudo certbot --nginx -d maheshkumar.blog -d www.maheshkumar.blog --force-renewal
  ```

**Site works on HTTP but not HTTPS**
- Firewall blocking port 443: `sudo ufw allow 443/tcp`
- Nginx not reloaded: `sudo systemctl reload nginx`

**"Too many certificates already issued"**
- Let's Encrypt rate limit (5 per week). Wait or use staging: `--staging`

---

## After Certbot Succeeds

If you want to use the full config from this repository (with custom security headers and redirects):

```bash
# Replace the config Certbot modified
sudo cp server/nginx.conf /etc/nginx/sites-available/maheshkumar.blog

# Test and reload
sudo nginx -t
sudo systemctl reload nginx
```

The repository's `server/nginx.conf` includes:
- HTTP → HTTPS redirect
- www → non-www redirect
- Security headers
- Modern SSL settings

---

## Testing Your SSL Configuration

Check your SSL setup quality:
- Visit: https://www.ssllabs.com/ssltest/
- Enter your domain
- Should get an "A" or "A+" rating

---

## Summary

Your site now has:
- ✓ Free SSL certificate from Let's Encrypt
- ✓ HTTPS enabled (padlock in browser)
- ✓ Auto-renewal configured
- ✓ HTTP traffic redirected to HTTPS

Certificates renew automatically every 60 days. You'll get email notifications if renewal fails.
