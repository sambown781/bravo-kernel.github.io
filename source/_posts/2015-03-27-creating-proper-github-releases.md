title: Creating proper Github releases
date: 2015-03-27 19:30:00
categories:
- Git
tags:
- git
- github
---
Step-by-step instructions for creating Github Releases like the pros.

Assuming:

- no previous releases
- a ``master`` branch
- a more-or-less stable ``dev`` branch ready to be merged into master
- a ``VERSION.txt`` file holding release version

**Pro Tip:** keep release cycles short, release often!

## Creating the release

1. create ``VERSION.txt`` in your dev branch and fill it with the release number (e.g. 1.0-alpha1)
2. merge the master branch into dev
3. merge the dev branch back into master
4. go to the Github page holding your project and:
    - select <em>releases</em>
    - select <em>Draft a new release</em>
    - fill in 1.0-alpha1 as the <em>Tag version</em> 
    - target the master branch
    - doublecheck before pressing the "Publish release" button

That's all there's too it, your first release.

## Starting work on the next release

Now that the release is out, it's time to start work on the next one:

1. update ``VERSION.txt`` in the dev branch with the next release number (e.g. 1.0-alpha2)
2. commit and push change to dev
3. add features to your dev branch

Once you are ready to publish a new release... simply restart ``Creating the release`` but skip step 1.


