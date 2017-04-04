---
ID: 2051
post_title: >
  How To Add A Split Sidebar in the
  Genesis Framework
author: Gary Jones
post_date: 2013-09-24 13:00:44
post_excerpt: >
  Learn how you can enable automatic
  updates for your WordPress plugin (or
  theme) direct from GitHub, with just one
  line of code.
layout: post
permalink: https://gamajo.com/split-sidebar-genesis
published: true
fsb_social_twitter:
  - "9"
fsb_social_facebook:
  - "0"
fsb_social_google:
  - "0"
fsb_show_social:
  - "0"
fsb_social_linkedin:
  - "0"
yst_prominent_words_version:
  - "1"
---
<div class="alert alert-info">This tutorial was originally posted to http://code.garyjones.co.uk on 2011-08-12. The comments have been carried across to this site. It has been updated here with a more detailed description and improved code.</p></div>

<p>Visually splitting the primary sidebar in a <a href="http://genesis-theme-framework.com/">Genesis Framework</a> child theme into two smaller sidebars is a powerful design feature. It allows prominent important areas to apparently span a double width, before breaking down into narrower columns for other widget content.

<h2>Get A Split Sidebar</h2>

If you're thinking "How did I get a widget to span two sidebars?" or "What CSS do I use to split a sidebar?", then you're over-thinking the problem. We don't actually split a sidebar, but we introduce two new widget areas that are completely independent, and then just display them below the main one instead. Kudos to <a href="http://briangardner.com/">Brian Gardner</a> for originally suggesting this approach after I was trying to recreate the Custom Content Box above the two sidebars as it appeared in Thesis.

By using new widget areas, users get clear guidance on where their widget will appear without having to work out if the new widget will appear in the normal sidebar, or in one of the split sidebar areas.

Here's what we're trying to achieve (screenshot from an client site I did):

[caption id="attachment_2097" align="aligncenter" width="500"]<img src="https://gamajo.com/wp-content/uploads/genesis-split-sidebars.png" alt="Screenshot showing a split sidebar, with one sidebar split into two narrower sidebars" width="500" height="502" class="size-full wp-image-2097" /> One sidebar, split into two (each with their own widget area)[/caption]

Ready for some code?

<h2>PHP</h2>

Add the following to your child theme <code>function.php</code> file:

https://gist.github.com/GaryJones/1698319#file_functions.php

The first function registers the two widget areas. For more info about <code>genesis_register_sidebar()</code> see the StudioPress tutorial on <a href="http://my.studiopress.com/tutorials/register-widget-area" title="StudioPress: How to Register a Widget Area">How to Register a Widget Area</a> (login needed).

The function is attached to the <code>after_setup_theme</code> action hook at priority 5 so that the split sidebar widget areas appear after Genesis has registered the Primary and Secondary sidebars, but before the footer widget areas.

[caption id="attachment_2098" align="aligncenter" width="317"]<img src="https://gamajo.com/wp-content/uploads/split-sidebar-widget-areas.png" alt="Screenshot showing a list of widget areas" width="317" height="452" class="size-full wp-image-2098" /> Sensible registration of the widget areas can determine the order in which they appear on the Widgets screen.[/caption]

The second function handles the output of the widget areas. Here we use the <code>genesis_widget_area()</code> function. This function is a wrapper for the WordPress function <code>dynamic_sidebar()</code> but it sets up the default markup for before and after the widget area and calls two contextually-named action hooks before and after the <code>dynamic_sidebar()</code> call, making it more flexible for plugins or later code. You can see that the single argument being passed to each call is just the ID of the widget areas we registered in the first function.

The second function is attached to the <code>genesis_after_sidebar_widget_area</code> hook, which is what determines the location of the split sidebar output (after the Primary sidebar). If it was attached to the <code>genesis_before_sidebar_widget_area</code> then the split sidebar would be above the Primary sidebar. 
Attaching it to <code>genesis_before_sidebar_alt_widget_area</code> or <code>genesis_before_sidebar_alt_widget_area</code> would make it appear before or after the Secondary sidebar instead.

<h2>CSS</h2>

The CSS is relatively trivial but might need adapting to fit in to your existing theme styles. We give each widget area half of the available width, and float them so they fill in the space instead of the second one stacking underneath the first.

http://gist.github.com/GaryJones/1698319#file_style.css

If you need to target the left sidebar you can use the <code>.split-sidebars > .widget-area:first-child {}</code> selector, and <code>.split-sidebars > .widget-area:last-child {}</code> for the right sidebar.

<h2>Conclusion</h2>

You can now register new widget areas, and output them above or below the primary and secondary sidebars, giving a "split sidebar" appearance. If you have any questions, let me know in the comments.