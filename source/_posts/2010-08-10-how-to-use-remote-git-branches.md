title: How to use remote Git branches
alias: /blog/2010/08/how-to-use-remote-git-branches/index.html
date: 2010-08-10 14:39:59 +0200
categories:
- Git
tags:
- git
---

## Create a feature branch:
Create a local feature branch called "tooltips" and add it to the remote repository:

	# git branch -a
	# git checkout -b tooltips
	# git push origin tooltips

##Develop on the feature branch:

Make sure you are working in the local "tooltips" branch and make some file changes.

	# git branch -a
	# git status
	# git commit .
	# git push origin tooltips

##Merge feature branch into master:
**Please note:** Merging should be done from within the local master

	# git checkout master
	# git diff master tooltips
	# git merge tooltips
	# git push origin

##Clean up the feature branch:

Once the feature branch is completely merged into the master we can safely remove the local and remote feature branches:

	# git checkout master
	# git branch -d tooltips
	# git push origin :tooltips
