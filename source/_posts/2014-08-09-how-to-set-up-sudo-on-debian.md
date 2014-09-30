title: How to set up sudo on Debian
alias: /blog/2014/08/how-to-set-up-sudo-on-debian/index.html
date: 2014-08-09 09:24:40 +0200
categories:
- Linux
tags:
- debian
---

To install sudo and add <username\> to the sudoers group run:

	apt-get install sudo
	adduser <username> sudo

**Note:** <username\> must logout and login to effectuate new group membership (and thus sudo).
