# apache-conf-examples
apache2 virtual hosts conf examples

### Website Name = domain.com

Verify PORT 80 and 443 is open on machine

## Create Conf File

sudo mkdir -p /var/www/domain.com/public_html
sudo chown -R $USER:$USER  /var/www/domain.com/public_html
sudo touch /var/www/domain.com/public_html/index.html
sudo nano /etc/apache2/sites-available/domain.com.conf

paste below text
<VirtualHost *:80>
	ServerAdmin admin@domain.com
	ServerName domain.com
	ServerAlias www.domain.com
	DocumentRoot /var/www/domain.com/public_html

  ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

RewriteEngine on
RewriteCond %{SERVER_NAME} =www.domain.com [OR]
RewriteCond %{SERVER_NAME} =domain.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

Now Save File

### Go to folder and run command
cd /etc/apache2/sites-available
sudo a2ensite domain.com.conf 

### Add entry in /etc/hosts	
3X.2XX.XX5.X domain.com

### Restart apache2	
sudo systemctl restart apache2

## Generate Certbot SSL certificate	
### run command	
sudo certbot --apache

## Serve API via Virtual Hosts | Node Express
Server running on localhost 3000 port 
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo nano /etc/apache2/sites-available/domain.com.conf

<VirtualHost *:80>
	ServerAdmin admin@api.domain.com
	ServerName api.domain.com
	ServerAlias www.api.domain.com
	ProxyPreserveHost On	
	ProxyPass / http://localhost:3000/
	ProxyPassReverse / http://localhost:3000/

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

RewriteEngine on
RewriteCond %{SERVER_NAME} =www.api.domain.com [OR]
RewriteCond %{SERVER_NAME} =api.domain.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

Now Save File 
### Go to folder and run command
cd /etc/apache2/sites-available
sudo a2ensite api.domain.com.conf 

### Add entry in /etc/hosts	
3X.2XX.XX5.X api.domain.com

### Restart apache2	
sudo systemctl restart apache2


