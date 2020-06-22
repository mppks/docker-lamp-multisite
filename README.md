# Docker-LAMP-Multisite
LAMP server based on Docker for web development with the possibility of running multiple projects
### Create projects folders
    $ mkdir ./www/site1
### Configure virtual hosts in ./hosts/000-default.conf
	<VirtualHost *:80>
		ServerAdmin mymail@gmail.com
		ServerName site1
		ServerAlias www.site1
		DocumentRoot /var/www/site1
		<Directory />
			Options FollowSymLinks
			AllowOverride All
		</Directory>
		<Directory /var/www/site1>
			Options Indexes FollowSymLinks MultiViews
			AllowOverride All
			Order allow,deny
			Allow from all
			Require all granted
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/site1_error.log
		CustomLog ${APACHE_LOG_DIR}/site1_access.log combined
	</VirtualHost>

### Add virtual hosts names in docker-compose.yml **web** service section **extra_hosts** key
	extra_hosts:
	  - "site1:127.0.0.1"
	  - "site2:127.0.0.1"

### Configure local hosts file (e.g. /etc/hosts)
	127.0.0.1 site1
	127.0.0.1 site1

### Build images before starting containers in first time
	$ sudo docker-compose up --build

### or start up next time
	$ docker-compose up

### Run shell in your web service
	$ docker-compose exec -u pks web bash

### Install node modules and run scripts
	$ cd site1/
	$ npm install
	$ npm run watch

## Аccess to sites
**Virtual hosts** are available in the host system by ports: 8080, 3000, 3001 (e.g. http://site1:8080, or http://site1:3000 in case of using browserSync)

**phpMyAdmin** available by port: 8181 (e.g. http://localhost:8181), with login *root* and password *toor*