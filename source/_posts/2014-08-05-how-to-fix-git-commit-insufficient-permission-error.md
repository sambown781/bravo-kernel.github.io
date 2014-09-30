title: How to fix Git commit insufficient permission error
alias: /blog/2014/08/how-to-fix-git-commit-insufficient-permission-error/index.html
date: 2014-08-05 14:45:52 +0200
categories:
- Git
tags:
- git
---

When `git commit` gives this error:

    error: insufficient permission for adding an object to repository database .git/objects

take back ownership of all Git objects by runnning:

    cd <path-to-repo>
    cd .git/objects
    sudo chown -R <username>:<username> *
