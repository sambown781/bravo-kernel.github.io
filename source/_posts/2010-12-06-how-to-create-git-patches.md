title: How to create Git patches
alias: /blog/2010/12/how-to-create-git-patches/index.html
date: 2010-12-06 14:18:17 +0200
categories:
- Git
tags:
- git
---

This example will create a patch for the CakeDC Comments Plugin:

	# cd /app/plugins/comments
	# git fetch
	# git checkout master
	# git checkout master
	# git checkout -b local_fix


You are now working in your local branch "local_fix" so:

- make required changes to the code
- commit the changes
- create the patch

In this case:

	# git add controllers/comments_controller.php
	# git commit -m "Fix for broken Search plugin usage on admin_index page"
	# git format-patch origin/master


If all went well:

- there should be a file named "0001-Fix-for-broken-Search-plugin-usage-on-admin_index-page.patch" in your /app/plugins/comments directory
- send the file to the big boys for approval
