title: How to create a new CakePHP application using FriendsOfCake app-template
alias: /blog/2014/08/how-to-create-a-new-cakephp-application-using-friendsofcake-app-template/index.html
date: 2014-08-10 09:31:56 +0200
categories:
- CakePHP
tags:
- cakephp
- foc
- composer
---

This will create a new CakePHP application using the awesome [FriendsOfCake app-template](https://github.com/FriendsOfCake/app-template "FriendsOfCake app-template").

Make sure you have:

- installed Composer (see ["How to install Composer on Debian"](/blog/2014/08/how-to-install-composer-on-debian/ "How to install Composer on Debian"))
- created an empty Github repository (we'll call it Debby)

###Install app-template

	$ cd [path-to-repos]
	$ sudo apt-get install php5-mcrypt
	$ git clone https://github.com/FriendsOfCake/app-template.git friendsofcake/app-template

### Generate app

	$ composer -sdev create-project friendsofcake/app-template debby

### Save app

	$ cd debby
	$ git init
	$ git add .
	$ git add tmp/ -f
	$ git commit -m "Initial FriendsOfCake app-template"
	$ git remote add origin git@github.com:<gh-username>/debby.git
	$ git push -u origin master
