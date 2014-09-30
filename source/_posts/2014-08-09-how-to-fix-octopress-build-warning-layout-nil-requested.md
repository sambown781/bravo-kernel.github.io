title: How to fix Octopress build warning Layout nil requested
alias: /blog/2014/08/how-to-fix-octopress-build-warning-layout-nil-requested/index.html
date: 2014-08-09 10:18:57 +0200

categories:
- Web
tags:
- octopress
---

When rake_generate produces errors like this:

	Build Warning: Layout 'nil' requested in blog/tags/git/atom.xml does not exist.

Fix by replacing:


	layout: nil

with:

	layout: null

in files:

- `source/atom.xml`
- `source/robots.txt`
- `source/_includes/custom/category_feed.xml`
