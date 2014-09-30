title: How to set up PHPCS with CakePHP coding standard on Ubuntu
alias: /blog/2014/09/how-to-set-up-phpcs-with-cakephp-coding-standard-on-ubuntu/index.html
date: 2014-09-01 10:41:23 +0200
categories:
- Web
tags:
- cakephp
- phpcs
- ubuntu
---

Install required package

	sudo apt-get install php-codesniffer

Install the [CakePHP Coding Standard](https://github.com/cakephp/cakephp-codesniffer "CakePHP Coding Standard")

	$ sudo pear channel-discover pear.cakephp.org
	$ sudo pear install cakephp/CakePHP_CodeSniffer


### Usage

	$ cd /var/repos
	$ cd <your-repo>
	$ phpcs app -p --extensions=php --standard=CakePHP
	...................................................
