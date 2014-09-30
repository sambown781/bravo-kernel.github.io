title: How to update an outdated local Git repo
alias: /blog/2012/06/how-to-update-an-outdated-local-git-repo/index.html
date: 2012-06-28 16:01:12 +0200
categories:
- Git
tags:
- git
---

When your local repository is behind/untracked this will:

- delete all local files that are no longer present in the remote repo
- replace all local files that have newer versions in the remote repo

<!-- break list -->

	git reset --hard HEAD
	git clean -f -d
	git pull
