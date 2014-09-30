title: How to install Octopress with GSL on Debian
alias: /blog/2014/08/how-to-install-octopress-with-gsl-on-debian/index.html
date: 2014-08-09 09:25:23 +0200
categories:
- Linux
tags:
- debian
- octopress
- wheezy
---

This will install Octopress with GSL (for 10x faster LSI support) on Debian Wheezy in 5 minutes.

Make sure you have created a [Github-pages](https://pages.github.com/) repository first.

## Install required packages

	sudo apt-get update
	sudo apt-get install ruby ruby-dev ruby-gsl build-essential

## Install GSL

We enable GSL to speed up page generation and remove this annoying `rake_generate` warning:

	Notice: for 10x faster LSI support, please install http://rb-gsl.rubyforge.org/

Compile by running:

	cd /usr/src
	sudo wget http://ftp.gnu.org/gnu/gsl/gsl-1.14.tar.gz
	sudo tar xvf gsl-1.14.tar.gz
	sudo gsl-1.14/configure
	sudo make
	sudo make install

Running this command should return `1.14`:

	gsl-config --version

## Install Octopress

	cd /<path-to-repos>
	git clone git://github.com/imathis/octopress.git octopress
	cd octopress

Open file `_config.yml` and add the following line:

	lsi: true

Prevent build warnings caused by missing 'nil' layout:

	touch source/_layouts/nil.html

## Install gems
Open file `Gemfile` and add the following lines to the development group:

	gem 'execjs'
	gem 'therubyracer'
	gem 'gsl'

Install gems by running:

	sudo gem install bundler
	bundle install

## Install Rake

Open file `Rakefile` and replace line 256:

	Bundler.with_clean_env { system "git pull" }

with:

	Bundler.with_clean_env { system "git pull origin #{deploy_branch}" }

Set up Rake by running:

	rake install
	rake setup_github_pages

## All done

That's all there's to it. You are now

- in Git branch `source` (never work in `master`!)
- ready to create your first post

## Reinstalling?

See ["How to reinstall Octopress with Octostrap3 theme on Debian"](/blog/2014/08/how-to-reinstall-octopress-with-octostrap3-theme-on-debian "How to reinstall Octopress with Octostrap3 theme on Debian")
