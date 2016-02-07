---
ID: 2113
post_title: >
  How to Add Post Type Counts to the
  WordPress Dashboard
author: Gary Jones
post_date: 2013-12-26 13:29:44
post_excerpt: ""
layout: post
permalink: https://gamajo.com/dashboard-glancer
published: true
fsb_show_social:
  - "0"
yourls_shorturl:
  - http://gmj.to/pv
fsb_social_twitter:
  - "1"
fsb_social_facebook:
  - "0"
fsb_social_google:
  - "0"
fsb_social_linkedin:
  - "0"
---
<p>When a plugin adds one or more custom post types (CPTs), a nice little addition is to give a count of the entries on the dashboard, as a summary, where the number of posts and pages is already listed. Doing it for one CPT is easy enough; doing it for multiple CPTs is an obvious case for re-using code, and doing it for CPTs across multiple plugins  makes a case for a separate library. Enter <em>Gamajo Dashboard Glancer</em>.
<!--more--></p>

<h2>The Gamajo Dashboard Glancer Library</h2>

The <a href="https://github.com/GaryJones/Gamajo-Dashboard-Glancer">Gamajo Dashboard Glancer</a> project centres around a single class, <code>Gamajo_Dashboard_Glancer</code>, which can be included within your CPT plugin. You then instantiate this class, and register post types to it, when you want to show the counts.

[gist id="8133765" file="my-plugin1.php"]

See the project README for more code examples.

The class supports adding multiple CPTs at once, along with multiple statuses, including custom statuses, so there's a lot of flexibility. The output becomes a list item, and will be linked, if the current user has the capability to edit entries.

The project also includes a second class that extends the first, to provide the appropriate markup for the table-based markup of the Right Now dashboard widget for WordPress 3.7 and earlier.

[gist id="8133765" file="my-plugin2.php"]

With a small bit of styling (not included within the class), you can make your CPT icons appear next to the counts as well:

<img src="https://gamajo.com/wp-content/uploads/at-glance-widget.png" alt="Screenshot of the At a Glance widget, showing several extra custom post types registered" width="454" height="184" class="aligncenter img-border" />

<a href="https://github.com/GaryJones/Gamajo-Dashboard-Glancer">Get Gamajo Dashboard Glancer class on GitHub</a>.