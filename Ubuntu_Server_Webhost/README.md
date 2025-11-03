DETAILED SUMMARY OF SERVER SETUP AND CONFIGURATION
Step	Task	Description<br>
1	Installed Apache	Apache2 was installed to serve web content over HTTP.<br>
2	Installed PHP	Installed PHP 8.3 and modules to support WordPress.<br>
3	Installed MySQL Server	MySQL was installed to manage databases.<br>
4	Created MySQL Database and User	Created a database cbwp_db and user wesadmin with full privileges for WordPress.<br>
5	Installed WordPress	Used wget to download and extract the latest WordPress release into /var/www/html.<br>
6	Configured wp-config.php	Edited to include custom DB name, user, and password.<br>
7	Imported existing WordPress site	Imported content from a previous host; homepage worked, but subpages returned 404 errors initially.<br>
8	Fixed permalinks and .htaccess issues	Enabled mod_rewrite, changed Apache config to allow .htaccess overrides, regenerated permalinks.<br>
9	Set up Apache Virtual Hosts	Created /etc/apache2/sites-available/site1.local.conf and site2.local.conf to host multiple sites on a single IP.<br>
10	Created Document Roots	Made separate folders: /var/www/site1.local and /var/www/site2.local.<br>
11	Mapped custom domains via /etc/hosts	Edited /etc/hosts on the client to map site1.local and site2.local to the server’s LAN IP 192.168.51.116.<br>
12	Validated setup via curl and browser	Verified working configurations with curl, fixed browser issues with DNS by updating local hosts file.<br>
<br><br>
TERMINAL COMMANDS USED & WHAT THEY DID<br>
Command	Purpose<br>
sudo apt update	Refreshes the package list.<br>
sudo apt install apache2	Installs the Apache web server.<br>
sudo apt install php libapache2-mod-php php-mysql	Installs PHP and MySQL support for PHP.<br>
php -v	Checks the installed PHP version.<br>
sudo apt install mysql-server	Installs MySQL database server.<br>
sudo mysql	Opens MySQL shell as root via socket authentication.<br>
CREATE DATABASE cbwp_db ...;	Creates a new WordPress database.<br>
CREATE USER 'wesadmin'@'localhost' IDENTIFIED BY 'yourpassword';	Creates a new MySQL user.<br>
GRANT ALL PRIVILEGES ON cbwp_db.* TO 'wesadmin'@'localhost';	Grants full access to the user for the WordPress database.<br>
FLUSH PRIVILEGES;	Applies permission changes in MySQL.<br>
wget https://wordpress.org/latest.tar.gz	Downloads the latest WordPress version.<br>
sudo tar -xvzf latest.tar.gz	Extracts the WordPress archive.<br>
sudo mv wordpress/* .	Moves extracted files to web root.<br>
sudo chown -R www-data:www-data /var/www/html	Sets Apache ownership to the WordPress files.<br>
sudo cp wp-config-sample.php wp-config.php	Creates the config file for WordPress.<br>
sudo nano wp-config.php	Edits the config file to match DB credentials.<br>
sudo a2enmod rewrite	Enables Apache’s URL rewriting module.<br>
sudo nano /etc/apache2/sites-available/000-default.conf	Edits Apache’s default site config.<br>
sudo systemctl restart apache2	Restarts Apache to apply changes.<br>
apache2ctl -S	Displays current virtual host setup.<br>
sudo mkdir -p /var/www/site1.local	Creates a new document root for a second site.<br>
sudo nano /etc/apache2/sites-available/site1.local.conf	Creates a new virtual host file.<br>
sudo a2ensite site1.local.conf	Enables the new virtual host.<br>
sudo systemctl reload apache2	Reloads Apache to apply virtual host changes.<br>
curl http://site1.local	Verifies that the virtual host returns the expected content.<br>
ping site1.local	Tests if the domain resolves to the server IP.<br>
sudo nano /etc/hosts	Edits the local DNS mapping for fake domains.<br>
<br><br>
ERRORS & FIXES ALONG THE WAY
Problem	Cause	Solution
ERROR 1045 (28000): Access denied for user	Wrong or non-existent MySQL user / missing privileges	Re-created user with correct privileges using MySQL shell.
mysql> prompt showed -> and command hung	Forgot to end SQL command with a semicolon (;)	Added missing ; or exited using Ctrl+C and retyped command.
Apache pages gave 404 on all non-homepage URLs	.htaccess file not working; mod_rewrite not enabled; AllowOverride not set	Enabled mod_rewrite, updated Apache conf to AllowOverride All, and regenerated permalinks in WordPress admin.
Apache served only the default site	Virtual hosts were not properly configured or missing ServerName	Ensured <VirtualHost *:80> was used and ServerName matched requested domain.
Browser said “Hmm. We’re having trouble finding that site.”	The browser’s OS couldn’t resolve site1.local to the server’s IP	Added site1.local to /etc/hosts on the client machine pointing to the server IP.
Bash error -bash: !: event not found	Used an unescaped exclamation point in echo command	Switched to single quotes or escaped the ! to avoid history expansion error.
Apache apache2ctl -S showed default binding to 127.0.1.1	Default config had 127.0.1.1 as fallback virtual host	Updated virtual hosts to use <VirtualHost *:80> and optionally set ServerName localhost globally to suppress warning.
