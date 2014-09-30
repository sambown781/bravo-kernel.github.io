title: How to install Composer on Debian
alias: /blog/2014/08/how-to-install-composer-on-debian/index.html
date: 2014-08-10 09:27:48 +0200
categories:
- Web
tags:
- debian
- wheezy
- composer
---

To make Composer (globally) available on Debian:

	$ cd /usr/src
	$ sudo apt-get install curl php5-cli
	$ curl -sS https://getcomposer.org/installer | sudo php
	$ sudo mv composer.phar /usr/bin/composer

Verify installation:

	$ composer --version
	Composer version 4ecdbf89c4a3d1e5dfe73c57e3202a5e2a18c87e 2014-07-28 20:12:27
