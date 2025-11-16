# Portfolio Index Project: Implementation Blueprint & LLM Prompts

## Overview
This document provides a complete implementation plan for building a minimal portfolio index page with Nginx and HTTPS configuration. The project is broken into small, safe, incremental steps with ready-to-use prompts for an LLM code generator.

---

## Phase 1: Planning & Architecture

### High-Level Blueprint

1. **Repository Structure Setup** - Create folder hierarchy
2. **Core HTML Page** - Build minimal index with centered layout
3. **Styling System** - Implement responsive CSS with system fonts
4. **Nginx Configuration** - Create production-ready config with placeholders
5. **Documentation** - Write setup guides for HTTPS and Nginx
6. **Repository Documentation** - Create comprehensive README
7. **Final Review** - Validate completeness against spec

### Chunk Breakdown (Round 1)

**Chunk A:** Repository skeleton + basic HTML structure  
**Chunk B:** Complete styling with responsive design  
**Chunk C:** Nginx configuration file  
**Chunk D:** HTTPS setup documentation  
**Chunk E:** Nginx setup documentation  
**Chunk F:** README and final integration  

### Chunk Breakdown (Round 2 - Granular Steps)

**Step 1:** Create folder structure only  
**Step 2:** Build HTML with name section (no styling)  
**Step 3:** Add links section to HTML  
**Step 4:** Create CSS file with centering basics  
**Step 5:** Add typography and responsive rules to CSS  
**Step 6:** Create nginx.conf with basic structure  
**Step 7:** Add HTTPS and redirect logic to nginx.conf  
**Step 8:** Write HTTPS setup guide (concept section)  
**Step 9:** Add commands to HTTPS setup guide  
**Step 10:** Write Nginx setup guide (installation section)  
**Step 11:** Complete Nginx setup guide (configuration section)  
**Step 12:** Create comprehensive README  
**Step 13:** Add spec.md and validate structure  

### Final Review of Step Sizing

✅ Each step is 5-15 minutes of focused work  
✅ No step requires understanding of future steps  
✅ Each step produces working, testable output  
✅ Steps build incrementally without jumps  
✅ All code is wired together by the end  

---

## Phase 2: LLM Implementation Prompts

Below are the prompts to feed sequentially to a code-generation LLM.

---

### Prompt 1: Repository Structure

```
Create the folder structure for a static portfolio website project with the following requirements:

- Root directory containing README.md and spec.md
- A "site/" folder for HTML, CSS, and JS files
- A "docs/" folder for documentation markdown files
- A "server/" folder for nginx configuration

Create only the directory structure and empty placeholder files. Do not add any content yet.

Expected output:
- Directory tree structure
- Empty files: README.md, spec.md, site/index.html, site/styles.css, site/script.js, docs/https-setup.md, docs/nginx-setup.md, server/nginx.conf
```

---

### Prompt 2: Basic HTML Structure with Name

```
Build the initial HTML structure for site/index.html with these requirements:

Content:
- DOCTYPE html5
- Proper head section with charset UTF-8 and viewport meta tag
- Page title: "Maheshkumar"
- Link to styles.css in the same directory
- Body containing a main element
- Inside main: an h1 element with the text "Maheshkumar"

Structure requirements:
- Semantic HTML5 elements
- No inline styles
- No JavaScript
- Clean, readable indentation

The page should be valid HTML but unstyled at this stage.
```

---

### Prompt 3: Add Links Section to HTML

```
Extend the existing site/index.html by adding a links section below the name heading.

Requirements:
- Add a nav element inside the main element, after the h1
- Inside nav, create an unordered list (ul)
- Add three list items (li), each containing an anchor tag (a)
- The three links should be:
  1. https://github.com/Maheshkumar-novice
  2. https://maheshkumar-novice.github.io/GoWeblogs/
  3. https://mahi-novice.bearblog.dev/
- Each anchor's href AND visible text should be the full URL
- All links should open in the same tab (no target="_blank")

Keep the HTML semantic and clean. Do not add any styling yet.
```

---

### Prompt 4: CSS Foundation with Centering

```
Create site/styles.css with foundational styling to center all content.

Requirements:
- Use CSS reset or normalize basics (margin, padding, box-sizing)
- Set body to use system font stack: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif
- Background: white (#ffffff)
- Text color: dark gray or black for readability
- Center all content horizontally using flexbox on the body element
- Body should be full viewport height
- Main element should be centered both horizontally and vertically

Do NOT style the links or list yet - keep them with browser defaults. Focus only on layout and typography foundation.
```

---

### Prompt 5: Complete CSS Styling and Responsiveness

```
Extend site/styles.css to complete the design with responsive behavior and proper spacing.

Requirements:
- Add generous vertical spacing between the h1 and nav (at least 3rem)
- Style the nav ul to remove default list styling (no bullets, no padding)
- Add spacing between list items (1.5rem minimum)
- Links should maintain browser default appearance: blue color, underline on hover
- Make the layout responsive:
  - Minimum supported width: 320px
  - Use relative units (rem, em) for spacing
  - Font sizes should scale appropriately on mobile
  - On very small screens, reduce spacing proportionally
- Add subtle line-height for better readability (1.6 recommended)

Keep the CSS minimal and clean. Total file should be well under 200 lines. No JavaScript is needed.
```

---

### Prompt 6: Nginx Configuration - Basic Structure

```
Create server/nginx.conf with the basic structure for serving a static website.

Requirements:
- Start with an HTTP server block (port 80)
- Server name: maheshkumar.blog www.maheshkumar.blog
- Root directory: use placeholder variable /var/www/DOMAIN_NAME/
- Default index file: index.html
- Add location block for serving static files
- Include basic access and error log paths with placeholders

Use comments to explain each section. Do NOT add HTTPS configuration yet - we'll add that in the next step.

Important: Use placeholder syntax for any paths that would be environment-specific:
- Use DOMAIN_NAME as placeholder in paths
- Use placeholder@example.com for any email references
```

---

### Prompt 7: Nginx Configuration - HTTPS and Redirects

```
Extend server/nginx.conf to add HTTPS support and www redirect.

Requirements:
- Add a new server block for HTTPS (port 443)
- Enable SSL with placeholder certificate paths:
  - ssl_certificate /etc/letsencrypt/live/DOMAIN_NAME/fullchain.pem;
  - ssl_certificate_key /etc/letsencrypt/live/DOMAIN_NAME/privkey.pem;
- Include recommended SSL settings (ssl_protocols, ssl_ciphers)
- Modify the HTTP server block to redirect all traffic to HTTPS
- Add a separate server block to redirect www.maheshkumar.blog to non-www
- Use 301 permanent redirects

Add comments explaining the security settings. Ensure the config is production-ready with placeholders.
```

---

### Prompt 8: HTTPS Setup Documentation - Concepts

```
Create docs/https-setup.md with conceptual explanation of HTTPS setup.

Content sections:
1. Overview: Brief explanation of why HTTPS is important
2. Prerequisites: What you need before starting (domain, DNS, server access)
3. DNS Configuration: Explain how to point domain to DigitalOcean droplet IP
   - Use placeholder xxx.yyy.zzz.aaa for IP addresses
4. What is Let's Encrypt and Certbot: Explain the tools conceptually
5. Certificate Lifecycle: Explain certificate issuance and renewal

Write in clear, instructional tone. This section should help someone understand the "why" before the "how". Do not include commands yet - we'll add those next.
```

---

### Prompt 9: HTTPS Setup Documentation - Commands

```
Extend docs/https-setup.md by adding the practical command sections.

Add the following sections with actual commands:

1. Installing Certbot on Ubuntu:
   - Package installation commands
   - Version verification

2. Running Certbot for Nginx:
   - Interactive certbot command
   - What information it will ask for
   - Use placeholder@example.com for email examples

3. Automatic Renewal:
   - Explain the renewal timer
   - Command to test renewal: certbot renew --dry-run
   - How to verify renewal is scheduled

4. Troubleshooting tips:
   - Common errors and solutions
   - Where to find logs (use placeholder paths)

Format commands in code blocks. Add warnings for any destructive operations. Keep tone helpful and beginner-friendly.
```

---

### Prompt 10: Nginx Setup Documentation - Installation

```
Create docs/nginx-setup.md focusing on initial server setup and Nginx installation.

Content sections:

1. Overview: What this guide covers
2. Prerequisites: Clean Ubuntu server, sudo access, domain configured
3. Installing Nginx:
   - Update package lists
   - Install nginx package
   - Enable nginx service to start on boot
   - Verify installation and service status
4. Understanding Nginx Directory Structure:
   - /etc/nginx/ overview
   - sites-available vs sites-enabled concept
   - Where logs are stored
5. Creating the Site Directory:
   - Command to create /var/www/maheshkumar.blog/
   - Set proper permissions
   - Explain ownership considerations

Use code blocks for all commands. Add explanatory text before each command section. Use placeholder DOMAIN_NAME where appropriate.
```

---

### Prompt 11: Nginx Setup Documentation - Configuration

```
Extend docs/nginx-setup.md with the configuration and deployment sections.

Add these sections:

1. Preparing the Configuration File:
   - Where to find nginx.conf in this repository (server/ folder)
   - Explain placeholder replacement needed
   - List of placeholders to replace with actual values

2. Deploying the Configuration:
   - Copy nginx.conf to /etc/nginx/sites-available/DOMAIN_NAME
   - Create symbolic link to sites-enabled
   - Remove default site configuration
   - Commands for each step

3. Testing and Activating:
   - nginx -t command to test configuration
   - systemctl reload nginx
   - How to view error messages if test fails

4. Copying Site Files:
   - How to copy site/ contents to /var/www/DOMAIN_NAME/
   - Verify permissions
   - Test by visiting domain

5. Integration with Certbot:
   - Note that HTTPS section should be added after running Certbot
   - Reference to https-setup.md
   - Order of operations

Keep the tone instructional. Add warnings before destructive commands.
```

---

### Prompt 12: Repository README

```
Create a comprehensive README.md for the repository root.

Required sections:

1. Title and Description:
   - Project name
   - One-sentence purpose: "Minimal portfolio index page for maheshkumar.blog"
   - Technology stack: HTML, CSS, Nginx, Let's Encrypt

2. Live Site:
   - Link to https://maheshkumar.blog
   - Brief description of what visitors will see

3. Repository Structure:
   - Display the folder tree
   - Brief explanation of each directory's purpose

4. Quick Start for Development:
   - How to view the site locally
   - Just opening index.html in browser is sufficient

5. Deployment Guide:
   - Link to docs/nginx-setup.md
   - Link to docs/https-setup.md
   - Mention the order: Nginx first, then HTTPS

6. Security Notes:
   - Remind users to replace all placeholders
   - Warn about not committing sensitive data

7. License (if applicable)

Keep README concise but complete. Use markdown formatting. Add a table of contents if README is long.
```

---

### Prompt 13: Final Integration and Validation

```
Complete the repository by adding spec.md and performing final validation.

Tasks:

1. Copy the provided specification document to spec.md in the root directory (content will be provided separately)

2. Create/update site/script.js:
   - Add a comment explaining this file is intentionally empty
   - Note: "No JavaScript required for this static portfolio page"

3. Validation checklist - verify that:
   - All folder structure matches specification
   - index.html is valid HTML5 and matches requirements
   - styles.css is under 200 lines and fully responsive
   - nginx.conf uses placeholders for all sensitive values
   - Both documentation files are complete with commands and explanations
   - README.md provides clear overview and links to docs
   - No sensitive information is present anywhere

4. Create a CHECKLIST.md file mapping each specification requirement to its implementation location

Output a final summary confirming all requirements are met and the repository is ready for deployment and open-source publishing.
```

---

## Phase 3: Execution Notes

### How to Use These Prompts

1. **Sequential Execution**: Feed prompts to the LLM one at a time, in order
2. **Validation Between Steps**: After each prompt, verify the output before proceeding
3. **Iteration**: If output doesn't meet requirements, refine and rerun that specific prompt
4. **Context Preservation**: Ensure the LLM has context of previous steps when working on later prompts

### Expected Timeline

- **Step 1-5**: Core website (30-45 minutes)
- **Step 6-7**: Server configuration (20-30 minutes)
- **Step 8-11**: Documentation (45-60 minutes)
- **Step 12-13**: Integration and validation (15-20 minutes)
- **Total**: ~2-3 hours for complete implementation

### Quality Gates

After completing all prompts, verify:

✅ HTML validates with W3C validator  
✅ CSS is clean and uses only system fonts  
✅ All URLs in HTML are clickable and correct  
✅ Nginx config passes `nginx -t` validation  
✅ Documentation has no sensitive information  
✅ README provides clear project overview  
✅ Repository structure exactly matches specification  

---

## Conclusion

This blueprint provides 13 granular, sequential prompts that build a complete portfolio website repository. Each step is independently testable, builds on previous work, and produces integrated, working code. No orphaned files or hanging references exist - everything is wired together progressively from folder structure through final validation.