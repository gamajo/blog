---
ID: 2821
post_title: >
  Better Organise your JavaScript with the
  Module Pattern
author: Gary Jones
post_date: 2014-07-23 20:13:13
post_excerpt: >
  An example of how to change typical
  JavaScript to use the module pattern,
  with named functions and a publicly
  accessible document ready callback.
layout: post
permalink: >
  https://gamajo.com/javascript-module-pattern-example
published: true
yourls_shorturl:
  - http://gmj.to/q1
---
One of my favourite technical authors Tom McFarlin recently published a piece about how to <a href="http://tommcfarlin.com/improve-javascript-in-wordpress/">Improve JavaScript in WordPress</a>. In it he introduces the object pattern as a way to namespace code, to avoid clashes just like we do in PHP with actual namespaces, classes or function name prefixes.

It's certainly better than a whole load of individual functions, or worse, a whole load of anonymous functions directly bound to events. But could we do better?

<h2>The Module Pattern</h2>

A quick search will provide more tutorials about the <a href="http://yuiblog.com/blog/2007/06/12/module-pattern/">Module pattern</a>, each with slightly different examples. To me, it's a way of encapsulating (hiding) all functionality in a module, and only revealing some of it to the outside world as needed.

I like to go one further, and avoid anonymous functions. Named functions are potentially re-usable, and help to self-document the code through the function names themselves.

<h2>Example</h2>

Let's look at a before and after example I recently did for a client. The important bit here isn't what the code is doing (or not, as there's a bug in the original code that apparently wasn't fixed in my rewrite), but how it is structured.

Here's the original code:

https://gist.github.com/GaryJones/6f9ac01ea090890726a6

We have everything wrapped in a jQuery document ready event (Line 1). A responsive menu icon is added (line 5), and this has an anonymous callback added to the click event (lines 7-9).

There's a few variables declared (line 13) before doing some callback on the window load and resize events. We don't know what functionality, until we've read the whole function (or the comment above the binding). We're hiding an element, and then assigning some widths and heights to those variables we declared outside of this function.

Then there's some more functionality attached (line 31) to three more window events that moves a bunch of things around. Finally (line 71) we wait for something and then slide it up.

That's probably very typical code found in themes. Nothing even gets defined to the JavaScript engine until the document ready event fires, so it delays what actions need to happen.

Let's see how it could be improved:

https://gist.github.com/GaryJones/15bc42d47a5ab216ee77

We start off by creating our module, <code>envy</code>, under which everything else is created. We assign to it an <a href="http://benalman.com/news/2010/11/immediately-invoked-function-expression/">immediately invoked function expression</a> (IIFE), that in this case, takes a single argument of <code>$</code>, to which we pass in <code>jQuery</code>, so we can use the typical shortcut for referencing jQuery. From the return at the end (line 95), <code>envy</code> ends up as a simple object that has properties referencing internal methods (more on that below).

We then add in the <code>'use strict';</code> statement, to tell browsers that we're not doing anything silly, and it should use the faster parsing engine that doesn't have to account for bad practice silliness. It applies to everything inside this function scope.

We then have those variables declared again (line 4), and then we have four named methods - <code>calculateSizes</code>, <code>doDisplacements</code>, <code>responsiveMenu</code> and <code>fadeOutStoreNotice</code>. Even without looking at the contents of these methods, the names make it a little clearer what they are concerned with than anonymous functions.

There's also a fifth method, called <code>ready</code> (line 84). This is what will be called on the document ready event. This is also where the advantage of named functions comes to light - the <code>calculateSizes()</code> and <code>doDisplacements()</code> functions are called immediately (on document ready), but are also bound to the window resize and resize &amp; scroll events respectively (using namespaced events as well to make unbinding easier), without having to re-define the whole function again.

For the technically astute, the functions aren't named. They are anonymous functions that are assigned to properties of the <code>envy</code> object, but as can be seen on lines 85-88, the end result is the same.

The final part of this module is to return an object (lines 95-97). This object has properties (only one here, line 96) which has a key, <code>ready</code> that becomes the public name of the method, and a value of the <code>envy</code> property, <code>ready</code>,  which has the function assigned to it.

Finally, this public method is used as the callback for the short version of the jQuery document ready event. When the file is first referenced, it can be parsed and have the <code>envy</code> IIFE called to define everything (saving time later), but no calculations or DOM adjustments are done until the document ready event is triggered.

The code is also more adherent to the <a href="http://make.wordpress.org/core/handbook/coding-standards/javascript/">WordPress JavaScript Coding Standards</a>.

<h2>Summary</h2>

Everyone will have their own preferences, but this version of the Module pattern makes most sense to me. Others <a href="http://snook.ca/archives/javascript/no-love-for-module-pattern">try to avoid it</a> for good reasons too. The choice is yours. You can even mix and match the approaches. For instance, Genesis admin JavaScript uses a named object as in Tom's example, but with named functions and a ready function callback.

The Module pattern is just one more approach to be aware of, that might be right for your situation.