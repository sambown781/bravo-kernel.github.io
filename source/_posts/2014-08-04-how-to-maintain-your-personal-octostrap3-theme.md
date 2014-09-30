title: How to maintain your personal Octostrap3 theme
alias: /blog/2014/08/how-to-maintain-your-personal-octostrap3-theme/index.html
date: 2014-08-04 11:22:47 +0200
categories:
- web
tags:
- octopress
- octostrap3
---

Make sure you have created a personal fork of the [Octostrap3 repository](https://github.com/kAworu/octostrap3 "Octostrap3 repository").

## Add theme as a Git submodule

Add personal Octostrap3 theme as a Git submodule:

	 cd <path-to-repos>/octopress
	 git submodule add git@github.com:bravo-kernel/octostrap3.git .themes/octostrap3
	 git add .gitmodules
	 git commit -m "Adding Octostrap3 as a Git submodule"
	 git push origin

## Update theme

	cd <path-to-repos>/octopress
	cd .themes/octostrap3
	git checkout master

Edit some files (see the [Octopress theme documentation](http://octopress.org/docs/theme "Octopress documentation on themes")).

## Install updated theme

Install updated theme (or it will not be used when when generating new pages):

	 cd <path-to-repos>/octopress
	 rake 'install[octostrap3]'

## Generate pages using updated theme

Generate and deploy gh-pages using the updated theme:

  	 cd <path-to-repos>/octopress
	 rake generate
	 rake deploy

## Save updated theme

	cd <path-to-repos>/octopress/.themes/octostrap3
	git add .
	git commit -m "Updating Octostrap3 theme"
	git push origin

## Update Git submodule

Point Git theme submodule to the latest version:

	cd <path-to-repos>/octopress
	git add .themes/octostrap3
	git commit -m "Updating octostrap3 submodule"
	git push origin
