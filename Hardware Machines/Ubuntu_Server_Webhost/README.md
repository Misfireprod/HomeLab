# 🖥️ Home Lab: Web Server & WordPress Deployment (Apache + PHP + MySQL)

**Objective:**  
Build and host multiple websites on a single Linux-based server using Apache, PHP, and MySQL. Demonstrated real-world configuration, DNS resolution, virtual host setup, and WordPress deployment with database integration and troubleshooting.

---

## ✅ Overview of Technologies Used

- **Operating System:** Ubuntu Server
- **Web Server:** Apache 2.4
- **Database Server:** MySQL 8
- **Scripting Language:** PHP 8.3
- **CMS:** WordPress (manual installation)
- **DNS Resolution:** Local `/etc/hosts` mapping
- **Networking:** Static LAN IP, virtual host domain mapping (`site1.local`)
- **Configuration Tools:** Nano, Apache site-available config files, systemctl, MySQL CLI

---

## 🛠️ Major Setup Steps

### 1. **Initial Web Stack Installation**
- Installed Apache web server and confirmed it served a test page on LAN IP.
- Installed PHP 8.3 with MySQL integration (`libapache2-mod-php`, `php-mysql`).
- Installed and secured MySQL database server.
- Created a dedicated database (`cbwp_db`) and user (`wesadmin`) for WordPress.

### 2. **WordPress Installation**
- Downloaded the latest WordPress release via `wget`.
- Extracted WordPress into the default Apache document root.
- Configured `wp-config.php` with custom database credentials.
- Completed WordPress setup through the browser interface.

### 3. **Virtual Host Configuration**
- Created Apache virtual host config files (`site1.local.conf`, `site2.local.conf`) to support multiple websites.
- Set up custom document roots at `/var/www/site1.local`, etc.
- Enabled virtual hosts with `a2ensite` and restarted Apache.
- Mapped domain names (`site1.local`) to local IP (`192.168.51.116`) using `/etc/hosts`.

### 4. **Migration and Directory Restructuring**
- Migrated the original WordPress site from `/var/www/html` to `/var/www/site1.local`.
- Corrected file ownership and permissions (`www-data`).
- Ensured `index.php` was prioritized in Apache directory index.

### 5. **Troubleshooting & Fixes**
- Resolved Apache directory listing issue by adjusting `DirectoryIndex`.
- Diagnosed and fixed broken CSS/images due to mismatched site URLs by forcing WordPress to use `http://site1.local` via `wp-config.php`.
- Ensured PHP files were parsed correctly by validating Apache PHP module was enabled (`php_module (shared)`).

---

## 🧪 Key Outcomes

- Successfully hosted a WordPress site under a named virtual host (`site1.local`) using local domain resolution.
- Implemented structured database access using dedicated MySQL users.
- Diagnosed and fixed multiple real-world server errors including:
  - Apache serving directory listings
  - PHP execution issues
  - WordPress URL mismatches causing broken assets
- Gained deeper understanding of Apache site configuration, URL rewriting, and `.htaccess` behavior.

---

## 💡 Skills Demonstrated

- Linux system administration (terminal-based)
- Web server configuration (Apache virtual hosts, modules)
- PHP and MySQL integration
- WordPress deployment and migration
- DNS simulation with `/etc/hosts`
- Troubleshooting web server issues (error logs, curl, ping, permissions)
- Secure database setup and privilege management

---

## 📋 Terminal Commands Reference (Select Highlights)

| Command | Description |
|---------|-------------|
| `sudo apt install apache2` | Installs Apache web server |
| `sudo apt install php libapache2-mod-php php-mysql` | Installs PHP 8.3 and MySQL connector |
| `sudo apt install mysql-server` | Installs MySQL server |
| `sudo mysql` | Opens MySQL CLI as root |
| `CREATE DATABASE cbwp_db;` | Creates WordPress database |
| `GRANT ALL PRIVILEGES ON cbwp_db.* TO 'wesadmin'@'localhost';` | Assigns user privileges |
| `wget https://wordpress.org/latest.tar.gz` | Downloads latest WordPress |
| `sudo nano /etc/apache2/sites-available/site1.local.conf` | Edits Apache virtual host config |
| `sudo a2ensite site1.local.conf` | Enables virtual host |
| `sudo systemctl restart apache2` | Restarts web server |
| `nano /etc/hosts` | Maps domain name to local IP |

---
