title: How to set up time, date and timezone on Debian
alias: /blog/2014/08/how-to-set-up-time-date-and-timezone-on-debian/index.html
date: 2014-08-10 13:48:16 +0200
categories:
- Linux
tags:
- debian
- wheezy
- time
---

###Display (incorrect) time, date and timezone

	$ date
	Wed Aug 13 23:50:50 MSK 2014

###Show active timezone

	$ diff -s /etc/localtime /usr/share/zoneinfo/`cat /etc/timezone`
	Files /etc/localtime and /usr/share/zoneinfo/Europe/Moscow are identical

###Fix incorrect timezone

	$ echo -e "Europe/Amsterdam" | sudo tee /etc/timezone
	$ sudo cp /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

###Install ntp client

	$ sudo apt-get install ntp
	Setting up ntp (1:4.2.6.p5+dfsg-2) ...
	[ ok ] Starting NTP server: ntpd.


###View corrected time, date and timezone

	$ date
	Thu Aug 14 21:55:03 CEST 2014
