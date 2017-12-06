---
ID: 2166
post_title: Why Use a Core Library Plugin?
author: Gary Jones
post_excerpt: >
  Why I use a core library plugin in
  WordPress sites, and how to create your
  own. Includes links to library classes
  and example plugins.
layout: post
permalink: https://gamajo.com/core-library-plugin
published: true
post_date: 2014-03-10 13:30:22
---
Developers know that duplicate code is a [bad thing][1]. Making changes to many areas of a project for it to stay consistent is an opportunity for bugs to creep in, and an unnecessary waste of time. For a recent refactoring project, the client had a theme functions file that was 2500 thousand lines long. It connected to two other databases, registered 11 post types, and had the other site customisations. My task was to move that out of theme and into discrete plugins which would persist if the client changed the theme. 
This post looks at how I approached handling the post types and taxonomies, and the customisations they needed. <!--more-->

## Registration of a Single Post Type {#registration} In a previous project, I worked on how best to register a single post type. From this, my object-orientated 

[`Gamajo_Registerable`][2] library emerged. With many post types and taxonomies to register, the design of the library makes a post type trivial to create. The concrete implementations only included code to meet internationalization requirements of strings. A developer can copy those strings into a new class and updated as necessary. The end result is a reduction in code, time saved now, and time saved in the future. I decided to split each post type up into its own plugin. While not necessary, it provided several advantages: 
*   Some post type plugins have related renderer code or a taxonomy registration as well. Grouping these related classes together means a user can activate or deactivate the plugins independently to get all or none of the functionality.
*   Future developers can better guess which plugin some code is likely to be in.
*   Plugins with just a registration are immediately available to have later additions. The client or future developers don't have to re-organise code themselves. Using 

`Gamajo_Registerable` would mean including the interface and classes in each post type plugin leading to code duplication. If I released a bug fix version of one of the classes, then having to update it in 11 plugins would be painful. So, I created a Core Library plugin. 
## Core Library Plugin {#plugin}

<img src="https://gamajo.com/wp-content/uploads/core-library-plugin.png" alt="File structure of a core library plugin" width="262" height="143" class="alignright" />Bill Erickson and others have written about "[core *functionality* plugins][3]" before. My core *library* plugin is different - even when activated, on its own, it has no effect. What it does do is make re-usable classes and interfaces available that other plugins can assume are present. I make the core library a must-use plugin. It requires the library files when PHP parses the plugin. The standard plugins do not instantiate *their* dependent classes until the `init` hook so there are no issues with load order. 
## Core Library Plugin Example {#example} I've created an example of a core library plugin on GitHub called 

[Core Library Plugin Example][4]. I've pulled it from a real project, so I've anonymised the site's details and replaced them with *X*. Included are the library classes from: 
*   [Gamajo-Registerable][2]
*   [Gamajo-Dashboard-Glancer][5]
*   [Gamajo-Template-Loader][6] What's not included yet is a check to see if PHP already knows about the interface and classes yet. This could happen if an individual plugin included them. It wasn't needed on my client's site on this occasion so 

`x_core()` remains quite simple. 
If multiple plugins need to contain some other functionality, then add it to your core library plugin. An example would be the [`class-gamajo-dashboard-rightnow.php`][7] class, or the Custom Meta Boxes library. The 

`x-core.php` and `x-core` directory would sit inside your `wp-contents/mu-plugins` directory (create it if it doesn't exist). 
## Standard Plugin Example {#standard-plugin-example} I've also created an example standard plugin, which depends on the core library plugin. I've called this the 

[Core Library Plugin Consumer Example][8]. As before, I pulled this from a real project, so I've re-branded the client as *X* again. The main plugin file contains a simple check to see if the `x_core()` function exists. If it doesn't, the plugin deactivates to avoid causing fatal errors. The theme can be setup with the [TGM Plugin Activation][9] class so all dependent plugins activate if `x_core()` is present. That's a topic for another day. The plugin registers an *offer* post type, an *offer type* taxonomy, and show the offers within a Genesis child theme. The three classes keep everything as separate objects: a Post Type object, a Taxonomy object and a Renderer object. A plugin could add an Admin or UI object to amend post type columns or filters, or a Meta object for handling meta data boxes for the post type etc. This object-orientated approach isn't specific to using a core library plugin. Having helpful classes in the library to add those features makes the other plugins smaller and consistent. The best example here is adding a row to the *At a Glance* dashboard widget. The library class only needs a post type and status from the standard plugin to work. 
## Conclusion {#conclusion} I see the main benefits of extracting out code as reduced maintenance, and a promotion of cleaner code. What are your thoughts on using a core library plugin?

 [1]: https://en.wikipedia.org/wiki/Don't_repeat_yourself
 [2]: https://github.com/GaryJones/Gamajo-Registerable
 [3]: https://www.billerickson.net/core-functionality-plugin/
 [4]: https://github.com/GaryJones/Core-Library-Plugin-Example
 [5]: https://github.com/GaryJones/Gamajo-Dashboard-Glancer
 [6]: https://github.com/GaryJones/Gamajo-Template-Loader
 [7]: https://github.com/GaryJones/Gamajo-Dashboard-Glancer/blob/master/class-gamajo-dashboard-rightnow.php
 [8]: https://github.com/GaryJones/Core-Library-Plugin-Consumer-Example
 [9]: http://tgmpluginactivation.com