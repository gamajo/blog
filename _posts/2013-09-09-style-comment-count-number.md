---
ID: 299
post_title: >
  How to Style a Comment Count Number in
  the Genesis Framework
author: Gary Jones
post_date: 2013-09-09 13:00:07
post_excerpt: >
  Add a background image or other styles
  to improve the appearance of the comment
  count number in the post info in a
  Genesis child theme.
layout: post
permalink: >
  https://gamajo.com/style-comment-count-number
published: true
yourls_shorturl:
  - http://gmj.to/69
tutorials:
  - "14"
fsb_social_twitter:
  - "0"
fsb_social_facebook:
  - "0"
fsb_social_google:
  - "4"
fsb_show_social:
  - "0"
fsb_social_linkedin:
  - "0"
---
<div class="alert alert-info"><p>This tutorial was originally posted to http://code.garyjones.co.uk on 2010-08-25. The comments have been carried across to this site. It has been updated here for HTML5 child themes.</p></div>

If you're using a Genesis child theme and need to style the comment count number for a post so that it's different from the word "Comment(s)", then there are two steps we need to take. First, we need to add some extra markup around the number part, and second, add some CSS to style that markup.

<h2>Comment Count Number?</h2>
The comment count is the link and number that appear in the byline for each post in most Genesis Framework child themes, after the date and author. It acts as a shortcut to jump straight down to the comments for people who have already read the post, but want to see what new feedback there might be.

<h2>Markup</h2>

Add the following to your <code>functions.php</code> file:

https://gist.github.com/GaryJones/1708175#file_functions.php

What we're doing here is editing the Genesis <code>[[post_comments]]</code> shortcode output that you may be using within another function, or in use by your child theme already. We use a regular expression to target the right bit of output, then replace it with similar code that has the extra markup in it. We then finish the filter by returning our amended output.

The output would be something like:

https://gist.github.com/GaryJones/1708175#file_output.html

<h2>Styles</h2>
Now that's done, we can use that extra span to target the CSS.

http://gist.github.com/GaryJones/1708175#file_style.css

In this case, we've simply made the text larger for the comment count number, but you can add in other styles as needed. You can even give the number a background image, and not the "Comment" part:

<img src="https://gamajo.com/wp-content/uploads/comment-count-number.png" alt="Screenshot of a comment count number with a speech bubble background image" width="116" height="60" class="aligncenter size-full wp-image-2101" />

<h2 id="zero-comments">It's Not Working For Zero Comments!</h2>

The default output for more than one comment in Genesis is "&lt;number> Comments".

The default output for exactly one comment is "1 Comment".

The default output for exactly one comment is "Leave a Comment".

Can you spot the problem?

The inconsistent lack of a comment count number when there are no comments means that the regular expression in the first PHP function won't match and therefore won't do the replacement that adds in our markup. Luckily, the <code>[[post_comments]]</code> shortcode has an attribute that allows you to set the format when there are <code>zero</code> comments. (You can also do it for <code>one</code> comment and <code>more</code> than one comment). You can add this attribute via Genesis Simple Edits plugin, or just filter in it via the <code>functions.php</code> file:

https://gist.github.com/GaryJones/1708175#file_functions_.php

<h2>Conclusion</h2>
You can now add markup around the comment count number, and let your creative juices flow about how interesting you can make it look.