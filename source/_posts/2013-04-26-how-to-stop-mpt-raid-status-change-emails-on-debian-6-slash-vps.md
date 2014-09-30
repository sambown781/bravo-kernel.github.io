title: How to stop mpt raid status change emails on Debian 6/VPS
alias: /blog/2013/04/how-to-stop-mpt-raid-status-change-emails-on-debian-6-slash-vps/index.html
date: 2013-04-26 13:47:32 +0200
categories:
- Linux
tags:
- debian
---

I was getting flooded with error emails on my Debian 6 server running on a hosted VPS:

    Subject: info: mpt raid status change on dev01

    This is a RAID status update from mpt-statusd.  The mpt-status program reports that one of the RAIDs changed state:

Since I'm not using RAID... running this as root will stop the emails:

	# /etc/init.d/mpt-statusd stop
 	# echo RUNDAEMON=no > /etc/default/mpt-statusd

As an alternative to step 2: use package rcconf  to deactivate this service on startup in all runlevel.
