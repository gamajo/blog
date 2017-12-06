---
ID: 2084
post_title: >
  Automatically Update Your Plugin From
  GitHub With One Line Of Code
author: Gary Jones
post_excerpt: >
  Learn how you can enable automatic
  updates for your WordPress plugin (or
  theme) direct from GitHub, with just one
  line of code.
layout: post
permalink: >
  https://gamajo.com/update-plugin-from-github
published: true
post_date: 2013-09-17 13:00:24
---
I really like using Git and GitHub to develop my plugins and contribute to other projects. I like being able to make cheap branches with 10-20 characters on the command-line. I like being able to make quick pull requests directly on GitHub without having to pull the project down locally first. I like to be able to raise Issues for bug fixes or improvements, and for others to do the same to my projects, all in one place. That makes Git and GitHub far more preferential to me as a developer and contributor, than the WordPress SVN plugin repo and support forums, and it's for that reason that more of <a href="https://github.com/GaryJones?tab=repositories" rel="me">my plugins</a> are only on GitHub, than the <a href="https://profiles.wordpress.org/GaryJ" rel="me">WP repo</a>. 
## WordPress Plugin Repo The 

[WordPress repo][1] does have a couple of advantages. The first is the integration in terms of being able to find and install new plugins direct from the WP admin. I personally don't use it for finding new plugins from the outset - I use a search engine to see if a plugin existed that appears to do what I want, before diving back into the WP admin to install it if it's on the WP repo, so I don't rate the "finding" aspect particularly highly. The integration of the initial install is great and it's hard to knock it. My plugins on GitHub all have instructions for three different ways you can install them, but they involve downloading a zip file, and either uploading the zip, extracting and uploading the files, or where the server set up is correct, just cloning my GitHub repo into the plugins directory. All of that is far more work than some want to undertake, and it's not a problem that's been solved yet. The second advantage that the WP repo has is being able to automatically *update* plugins from within the WP admin. WordPress checks the repo every 24 hours or so, and gives you a notification if there's an update available. When a plugin is not from the WP repo, WP doesn't know where it should be checking for updates, or where it should be downloading an updated packaged from. For plugins on GitHub, this particular problem has been solved, in two different ways. 
## Updating from GitHub

### Internal Class The first solution would be something like the 

[WordPress GitHub Plugin Updater][2] originally from Joachim Kudish. It works by each plugin project including a copy of the `updater.php` file, and instantiating the class inside of it with a config array of 11 values. The main advantage is that the plugin is self-contained in terms of updates. It doesn't require a reliance on anyone or anything else. The disadvantages are that it requires having a copy of that updater class in each and every plugin, and should improvements be made to the class, then all of those plugins may benefit from the improved class. From a purist's point of view, it also means including un-related functionality (updating from GitHub source) within a plugin that was created to do something else entirely, which just seems wrong. There's also the integration aspect - while most of the config array can be copied, pasted and tweaked, including that and the class when your plugin might only be a simple filter function seems over the top. 
### External Plugin The second solution is to let another plugin handle that updating from GitHub functionality, and make your plugin support it. It too has advantages and disadvantages that semi-mirror the first solution. Here, the advantage is that there's no heavy integration - no updater class to add, and very little setup. Functionality is kept separate from whatever purpose the plugin has, and there's no duplication (or variations) of that class in multiple plugins to worry about. The disadvantage is that for your plugin to be updateable, you're expecting a user to install another plugin. That's up to them of course, but if they've gone to the effort of installing your GitHub-only plugin in the first place, then there's a good chance they'll be happy to have one more plugin that can enable automatic updates for your plugin and those of other GitHub-sourced plugins. 

## Update Your Plugin From GitHub It's with this background knowledge then that I want to introduce how you can enable automatic updates for your GitHub-only plugin with one line of code (or three, if necessary). I've been contributing to a plugin, 

[GitHub Updater][3], that once installed and activated, allows plugins that support it to also be updateable direct from GitHub. It also supports updating themes from GitHub as well, but that's a topic for another day. The updates themselves are seamless - the notification appears when there's an update, and other than a required rename (using the [Filesystem API][4]) of the zip file to match the plugin directory, updating appears to follow the standard process, as you can see here: ![Output from GitHub Updater][5] The [updater plugin readme][6] has more detailed instructions, but it basically comes down to one extra line in your own plugin header tags - `GitHub Plugin URI: https://github.com/<i><owner></i>/<i><repo></i>`. That's it. The updater plugin will use that URL for an API call that can retrieve the content of the main plugin file on the repo. It then reads it to find what the remote version is before comparing it to the local version, and then giving a notification as necessary. The updater plugin can also handle a git-flow situation, when the default branch is not the one that should be used for version checking or downloads. In that case, `GitHub Branch: master` can be used, which can point to the master or other branch (this may change in the future to looking at the latest tag instead). There's also an access token line so that private repos can be checked against too, though I've not yet personally tested this is working as intended yet. 
## Supporting the GitHub Updater plugin You can see that adding support for this plugin is extremely simple - one to three lines of code that can be dropped into existing plugins without any backwards compatibility issues. If the end-user doesn't have the updater plugin installed and active, then those extra plugin headers become redundant comments and have no negative effect whatsoever. I'm in the process of adding support for all of my plugins (and will also look at removing my plugins from the WP repo so they just sit on GitHub), and I'd strongly encourage any developers with GitHub-sourced plugins to do the same. An updater for GitHub repos is never going to make it into WP core, but with enough plugins supporting it, it becomes beneficial for all developers who favour Git over SVN as it gives them a real choice, without unduly affecting the end-users.

 [1]: https://wordpress.org/plugins
 [2]: https://github.com/radishconcepts/WordPress-GitHub-Plugin-Updater
 [3]: https://github.com/afragen/github-updater
 [4]: https://codex.wordpress.org/Filesystem_API
 [5]: https://gamajo.wpengine.com/wp-content/uploads/github-updater-rename.png
 [6]: https://github.com/afragen/github-updater/blob/master/README.md