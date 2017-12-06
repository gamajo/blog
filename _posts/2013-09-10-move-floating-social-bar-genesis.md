---
ID: 2092
post_title: >
  Move the Floating Social Bar Outside of
  Entry Content in Genesis
author: Gary Jones
post_excerpt: "How to move the Floating Social Bar, a great sharing plugin, in Genesis so that it doesn't impact on your microdata or design."
layout: post
permalink: >
  https://gamajo.com/move-floating-social-bar-genesis
published: true
post_date: 2013-09-10 13:00:30
---
The [Floating Social Bar][1] plugin is a way for readers to easily share posts on their social networks - the key difference is that it's very fast, and doesn't front-load the loading of the all of the scripts for the social sites. At the time of writing, I'm using it on this site, and you can see it just above. In actual fact, it's in a very slightly different place to where it would be by default. You see, to make the plugin work for all themes, Thomas and Syed appended the FSB output to the top of the post content. And that's fine for most cases. Looking at a page with the Floating Social Bar enabled, in the [Google Structured Data Testing Tool][2], I could see that the textual output from the Floating Social Bar was being included within the `blogPosting` item text. [caption id="attachment_2093" align="aligncenter" width="823"]<img src="https://gamajo.com/wp-content/uploads/floating-social-bar-microdata-issue.png" alt="Screenshot of Google Structured Data Testing Tool showing the text value includes the textual output from the Floating Social Bar" width="823" height="262" class="size-full wp-image-2093" /> The text value includes the textual output from the Floating Social Bar[/caption] I didn't want this. I'm not sure if Google actually uses this microdata value in any specific way, but I wanted to at least try and clear it up. 
## Moving the Floating Social Bar in Genesis Here's the solution for Genesis child themes (add to 

`functions.php`). Other themes could do the same, except they'll need to pick their own specific action hook on which to add the Floating Social Bar back in. https://gist.github.com/GaryJones/6496730 The code itself is documented with a couple of notes. In Genesis child themes then, it simply moves the Floating Social Bar output from immediately after the `<div class="entry-content" itemprop="text">` tag to immediately before it. Unless your entry-content has a background colour or a top border (which might be another reason for moving it anyway), you shouldn't notice any visual difference. 
## Plugin Since I'm going to be using this across all the sites where I use Floating Social Bar and Genesis, I've turned this into a plugin. Feel free to fork and improve or tweak to your own preference. 

[Move Floating Social Bar in Genesis plugin on GitHub][3]

 [1]: https://wordpress.org/plugins/floating-social-bar/
 [2]: https://developers.google.com/structured-data/testing-tool/
 [3]: https://github.com/GaryJones/move-floating-social-bar-in-genesis