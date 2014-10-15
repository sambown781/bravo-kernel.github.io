title: Kitchen converge re-run error on Windows against Vagrant with synced folder
date: 2014-10-15 20:26:33
categories:
- Sysop
tags:
- chef-dk
- kitchen
- vagrant
---
## The case 
Running `kitchen converge` on Windows using Chef-DK against a .kitchen.yml vagrant server with a synced folder:

- succeeds the first time
- fails every following attempt stating there is <em>"No such file or directory"</em>

Strangely enough:

- the mentioned file does exist (both locally and remote)
- manually deleting the mounted folder solves the problem


## The error

Where `kitchen converge` would normally display <em>"Removing non-cookbook files before transfer"</em> it now produces the following errors.

**On screen:**

	-----> Starting Kitchen (v1.2.1)
	-----> Converging <default-ubuntu-1404>...
		   Preparing files for transfer
		   Resolving cookbook dependencies with Berkshelf 3.1.5...
	>>>>>> ------Exception-------
	>>>>>> Class: Kitchen::ActionFailed
	>>>>>> Message: Failed to complete #converge action: [No such file or directory - C:/Users/bravo-kernel/AppData/Local/Temp/default-ubuntu-1404-sandbox-20141014-5236-n5f95k/cookbooks/vagrant-hexo/blogs/default-ubuntu-1404/demo/node_modules/hexo-renderer-stylus/node_modules/nib/node_modules/stylus/node_modules/glob/node_modules/inherits/inherits_browser.js]
	>>>>>> ----------------------
	>>>>>> Please see .kitchen/logs/kitchen.log for more details
	>>>>>> Also try running `kitchen diagnose --all` for configuration

**.kitchen/logs/kitchen.log:**
	
	I, [2014-10-14T18:09:33.513287 #5236]  INFO -- Kitchen: -----> Starting Kitchen (v1.2.1)
	I, [2014-10-14T18:09:37.866034 #5236]  INFO -- Kitchen: -----> Converging <default-ubuntu-1404>...
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: ------Exception-------
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: Class: Kitchen::ActionFailed
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: Message: Failed to complete #converge action: [No such file or directory - C:/Users/bravo-kernel/AppData/Local/Temp/default-ubuntu-1404-sandbox-20141014-5236-n5f95k/cookbooks/vagrant-hexo/blogs/default-ubuntu-1404/demo/node_modules/hexo-renderer-stylus/node_modules/nib/node_modules/stylus/node_modules/glob/node_modules/inherits/inherits_browser.js]
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: ---Nested Exception---
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: Class: Errno::ENOENT
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: Message: No such file or directory - C:/Users/bravo-kernel/AppData/Local/Temp/default-ubuntu-1404-sandbox-20141014-5236-n5f95k/cookbooks/vagrant-hexo/blogs/default-ubuntu-1404/demo/node_modules/hexo-renderer-stylus/node_modules/nib/node_modules/stylus/node_modules/glob/node_modules/inherits/inherits_browser.js
	E, [2014-10-14T18:09:44.489123 #5236] ERROR -- Kitchen: ------Backtrace-------

## The cause

The reason is so obvious one might never consider it at all:

> The path to the non-cookbook file to be deleted exceeds the maximum of 255 characters supported by the Windows platform causing the kitchen delete command to fail.

## The solution

Since there does not seem to be a solid solution (yet) resort to one of the following workarounds:

- shortening the path by moving the user's sandbox directory to a higher level (e.g. directly unde	r C:)
- shortening the path by changing the directory structure on the remote node