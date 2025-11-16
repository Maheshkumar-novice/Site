# Repository Specification — Portfolio Index + HTTPS + Nginx

This document defines the complete structure and implementation requirements for the repository that powers the index page for **maheshkumar.blog**.  
It is written to guide developers who will build or maintain this project.

---

## 📂 Repository Structure

```

/
README.md
site/
index.html
styles.css
script.js      ← (intentionally empty or omitted; no JS required)
docs/
https-setup.md
nginx-setup.md
server/
nginx.conf
spec.md          ← (this file)

```

---

## 🌐 Live Website Requirements

- Served via **Nginx on an Ubuntu DigitalOcean droplet**
- Uses **Let’s Encrypt + Certbot** for HTTPS certificates
- **Primary domain:** `https://maheshkumar.blog`
- **Redirect:** `https://www.maheshkumar.blog` → non-www
- Static files served locally from the droplet (no proxying to GitHub Pages)

---

## 🎨 Index Page (site/index.html)

### Purpose
A minimal personal landing page that introduces **Maheshkumar** and provides links to three writing platforms.

### Content Requirements
1. Intro section:
   - Display only the name **“Maheshkumar”**
   - No profile picture, tagline, or description

2. Links section:
   - Display three clickable URLs using the full URL text:
     - `https://github.com/Maheshkumar-novice`
     - `https://maheshkumar-novice.github.io/GoWeblogs/`
     - `https://mahi-novice.bearblog.dev/`

### Layout Rules
- All content **centered** horizontally
- Vertical single column layout
- Generous spacing between elements
- Mobile responsive on screens >= 320px width

### Typography & Style
- **System font stack only**  
  `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`
- **Light theme:** white background, dark text
- **Links:** browser default style  
  (blue text with underline on hover; no custom styling)
- No JavaScript

### CSS
- Implement responsiveness using only CSS  
- Use flexbox or margin auto for centering
- Keep total CSS small (< 200 lines recommended)

---

## 📄 Documentation Pages (docs/)

### 1. `https-setup.md`
Content requirements:
- Explain HTTPS setup conceptually
- Include commands for each step
- Topic coverage:
  - Domain DNS configuration for DigitalOcean Droplet IP
  - Installing Certbot
  - Running Certbot for Nginx
  - Automatic certificate renewal
- Use placeholders instead of sensitive values  
  Example: `example@placeholder-mail.com`

### 2. `nginx-setup.md`
Content requirements:
- End-to-end walkthrough for first-time server configuration
- Topic coverage:
  - Installing Nginx on Ubuntu
  - Directory to store site files (e.g., `/var/www/maheshkumar.blog/`)
  - Copying `nginx.conf` into `/etc/nginx/sites-available/`
  - Enabling via symlink to `/etc/nginx/sites-enabled/`
  - Testing config & reloading Nginx
  - Integration notes with Certbot
- No sensitive values — always use placeholders

---

## ⚙️ Nginx Config File (server/nginx.conf)

A **full ready-to-paste config** with placeholders.

### Required behavior
- Serve static HTML content from:
```

/var/www/maheshkumar.blog/   ← path must be a placeholder variable in repo

```
- Primary domain:
```

maheshkumar.blog

```
- Redirect from:
```

[www.maheshkumar.blog](http://www.maheshkumar.blog) → maheshkumar.blog

```
- HTTPS configuration using Let’s Encrypt
- Paths to certificate files must use placeholders like:
```

/etc/letsencrypt/live/maheshkumar.blog/fullchain.pem
/etc/letsencrypt/live/maheshkumar.blog/privkey.pem

```

---

## 🧹 Public Repo Safety Rules

This repository must **never** include:
- Private IPs
- Email addresses not marked as placeholders
- Real Certbot log excerpts
- Actual certificate files
- Credentials or SSH keys

When referencing sensitive data, use placeholder syntax, for example:
```

xxx.yyy.zzz.aaa  ← droplet IPv4 address
/me@example.com  ← email address placeholder

```

---

## ✔️ Completion Requirements

| Component | Status requirement |
|----------|--------------------|
| index.html | Matches all layout & styling requirements |
| styles.css | Clean & minimal, responsive, system fonts only |
| docs/https-setup.md | Concept explanation + commands |
| docs/nginx-setup.md | End-to-end walkthrough |
| server/nginx.conf | Fully valid + placeholders |
| README.md | Repo purpose + links to docs |
| spec.md | Included unchanged |

When all items above are complete, the repository is considered ready for deployment & open-source publishing.

---