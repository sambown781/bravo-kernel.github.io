title: How to reinstall Octopress with Octostrap3 theme on Debian
alias: /blog/2014/08/how-to-reinstall-octopress-with-octostrap3-theme-on-debian/index.html
date: 2014-08-14 17:44:33 +0200
categories:
- Web
tags:
- octopress
- octostrap3
- debian
---

###Reinstall Octopress

	$ cd [path-to-repos]
	$ git clone git@github.com:[your-github-pages-repo.git] octopress
	$ cd octopress
	$ git checkout source
	$ rake setup_github_pages

### Re-enable theme submodule

	$ git submodule init
	$ git submodule update
	$ cd .themes/octostrap3
	$ git checkout master
