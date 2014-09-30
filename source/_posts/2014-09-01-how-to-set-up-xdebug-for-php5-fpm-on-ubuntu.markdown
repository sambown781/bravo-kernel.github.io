title: How to set up Xdebug for PHP5-FPM on Ubuntu
alias: /blog/2014/09/how-to-set-up-xdebug-for-php5-fpm-on-ubuntu/index.html
date: 2014-09-01 10:36:52 +0200
categories:
- Web
tags:
- xdebug
- php
- ubuntu
---

Install required packages:

	$ sudo apt-get install php5-dev php-pear
	$ sudo pecl install xdebug

Open file `/etc/php5/fpm/php.ini` and add the following line to the "Dynamic Extensions" section:

	zend_extension="/usr/lib/php5/20121212/xdebug.so"

Restart PHP:

	$ sudo service php5-fpm restart
