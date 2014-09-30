title: How to install a Debian LEMP server
alias: /blog/2014/08/how-to-install-a-debian-lemp-server/index.html
date: 2014-08-10 09:26:05 +0200
categories:
- Linux
tags:
- debian
- wheezy
- nginx
---

This will install a LEMP server with nginx, MySQL and PHP-fpm on Debian Wheezy in 5 minutes.

### Install required packages

	$ sudo apt-get update
	$ sudo apt-get install mysql-server php5-mysql nginx php5-fpm

### Enable PHP

Open file `/etc/nginx/sites-available/default` as root and replace line

	index index.html index.htm;

with :

	index index.php index.html index.htm;

In the same file, replace the `php$` section with:

	location ~ \.php$ {
		try_files $uri =404;
		fastcgi_pass unix:/var/run/php5-fpm.sock;
		fastcgi_index index.php;
		fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
		include /etc/nginx/fastcgi_params;
	}

### Round up

Create a simple index.php:

	$ echo -e '&lt;?php echo "PHP index"; ?&gt;' | sudo tee /usr/share/nginx/www/index.php

Restart nginx:

	$ sudo /etc/init.d/nginx restart

### All done

Browse to your server, it should display `PHP index`.
