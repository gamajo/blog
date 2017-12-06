---
ID: 2051
post_title: >
  How To Add A Split Sidebar in the
  Genesis Framework
author: Gary Jones
post_excerpt: >
  Learn how you can enable automatic
  updates for your WordPress plugin (or
  theme) direct from GitHub, with just one
  line of code.
layout: post
permalink: https://gamajo.com/split-sidebar-genesis
published: true
post_date: 2013-09-24 13:00:44
---
<div class="alert alert-info">
  This tutorial was originally posted to https://code.garyjones.co.uk on 2011-08-12. The comments have been carried across to this site. It has been updated here with a more detailed description and improved code.</p>
</div>

Visually splitting the primary sidebar in a [Genesis Framework][1] child theme into two smaller sidebars is a powerful design feature. It allows prominent important areas to apparently span a double width, before breaking down into narrower columns for other widget content. 
## Get A Split Sidebar If you're thinking "How did I get a widget to span two sidebars?" or "What CSS do I use to split a sidebar?", then you're over-thinking the problem. We don't actually split a sidebar, but we introduce two new widget areas that are completely independent, and then just display them below the main one instead. Kudos to 

[Brian Gardner][2] for originally suggesting this approach after I was trying to recreate the Custom Content Box above the two sidebars as it appeared in Thesis. By using new widget areas, users get clear guidance on where their widget will appear without having to work out if the new widget will appear in the normal sidebar, or in one of the split sidebar areas. Here's what we're trying to achieve (screenshot from an client site I did): [caption id="attachment_2097" align="aligncenter" width="500"]<img src="https://gamajo.com/wp-content/uploads/genesis-split-sidebars.png" alt="Screenshot showing a split sidebar, with one sidebar split into two narrower sidebars" width="500" height="502" class="size-full wp-image-2097" /> One sidebar, split into two (each with their own widget area)[/caption] Ready for some code? 
## PHP Add the following to your child theme 

`function.php` file: https://gist.github.com/GaryJones/1698319#file_functions.php The first function registers the two widget areas. For more info about `genesis_register_sidebar()` see the StudioPress tutorial on [How to Register a Widget Area][3] (login needed). The function is attached to the `after_setup_theme` action hook at priority 5 so that the split sidebar widget areas appear after Genesis has registered the Primary and Secondary sidebars, but before the footer widget areas. [caption id="attachment_2098" align="aligncenter" width="317"]<img src="https://gamajo.com/wp-content/uploads/split-sidebar-widget-areas.png" alt="Screenshot showing a list of widget areas" width="317" height="452" class="size-full wp-image-2098" /> Sensible registration of the widget areas can determine the order in which they appear on the Widgets screen.[/caption] The second function handles the output of the widget areas. Here we use the `genesis_widget_area()` function. This function is a wrapper for the WordPress function `dynamic_sidebar()` but it sets up the default markup for before and after the widget area and calls two contextually-named action hooks before and after the `dynamic_sidebar()` call, making it more flexible for plugins or later code. You can see that the single argument being passed to each call is just the ID of the widget areas we registered in the first function. The second function is attached to the `genesis_after_sidebar_widget_area` hook, which is what determines the location of the split sidebar output (after the Primary sidebar). If it was attached to the `genesis_before_sidebar_widget_area` then the split sidebar would be above the Primary sidebar. Attaching it to `genesis_before_sidebar_alt_widget_area` or `genesis_before_sidebar_alt_widget_area` would make it appear before or after the Secondary sidebar instead. 
## CSS The CSS is relatively trivial but might need adapting to fit in to your existing theme styles. We give each widget area half of the available width, and float them so they fill in the space instead of the second one stacking underneath the first. http://gist.github.com/GaryJones/1698319#file_style.css If you need to target the left sidebar you can use the 

`.split-sidebars > .widget-area:first-child {}` selector, and `.split-sidebars > .widget-area:last-child {}` for the right sidebar. 
## Conclusion You can now register new widget areas, and output them above or below the primary and secondary sidebars, giving a "split sidebar" appearance. If you have any questions, let me know in the comments.

 [1]: https://genesis-theme-framework.com/
 [2]: https://briangardner.com/
 [3]: https://my.studiopress.com/tutorials/register-widget-area "StudioPress: How to Register a Widget Area"