---
ID: 2602
post_title: Comment Author Link Bug in Genesis 2.1.0
author: Gary Jones
post_excerpt: "There's a bug with the Comment Author Link in Genesis 2.1.0 and 2.1.1. (fixed in Genesis 2.1.2). This explains the problem and how you can fix it yourself to include the missing attributes. Without it, your site will have invalid markup and will be telling search engines that comment author links from your site to elsewhere should be included for ranking purposes."
layout: post
permalink: >
  https://gamajo.com/comment-author-link-bug-genesis
published: true
post_date: 2014-07-14 13:56:46
---
It's come to my attention about a bug in Genesis 2.1.0. It wasn't spotted until after Genesis 2.1.1 was released, and since that's the latest release, it still exists in sites with Genesis 2.1.0 or 2.1.1. This post explains the problem and how to fix it, so you're not waiting until Genesis 2.1.2 or 2.2.0 comes out. If you're competent, you can skip to the [TL;DR Fix][1]. **Update 2014-07-15**: This has now been fixed in Genesis 2.1.2, so ensure you're running at least that version to not be affected. 
## The Background {#background} In 2.0.*, for a HTML5 child theme, a comment author link would appear something like: 

<pre><code class="pre-scrollable">&lt;a href="https://genesischangelog.com" rel="external nofollow" itemprop="url"&gt;
    Gary Jones&lt;/a&gt;
</code></pre> Simple enough - an anchor link, that links my name to my site, and includes two attributes. The first is the 

`rel` attribute, with the important value of [`nofollow`][2]. This value has been recognised by Google et al for nearly 10 years. They introduced it as a way to [prevent comment spam][3]. Since search engine take into account which sites link to the one being ranked, the intent was to negate the benefit from someone posting spam comments all over the web, just to apparently have those high-ranking websites link to their little site. Google says: 
> From now on, when Google sees the attribute (rel="nofollow") on hyperlinks, those links won't get any credit when we rank websites in our search results.  The second attribute is `itemprop`. This has a value of `url` here, and is microdata for marking-up a [Person][4]. Not having it present is not as critical as the missing `rel` attribute. 
## The Comment Author Link Problem {#problem} The problem can be clearly seen in the comment author link markup for Genesis 2.1.0 and 2.1.1. It produces markup like: 

    <a href="https://genesischangelog.com/" comment-author-link="">Gary Jones</a>
     The link still points my name to my site, but the attributes have changed. Gone are the important 

`rel` and `itemprop` attributes. There's an attribute called `comment-author-link` instead, and this makes the markup for the page invalid. So what's going on here? The `comment-author-link` is our clue as to where to find the problem. In Genesis 2.1.0, one of the [changes][5] was to make more of the comment markup was passed through the Genesis Markup API. This is made up of two functions - `genesis_markup()` and `genesis_attr()`. They allow developers to filter markup and attributes as needed. The bug looks to be related to this. 
## The Fix {#fix}

*At this moment in time, it's not actually been confirmed as a bug and a bug fix by Copyblogger, but it's so clearcut I have no problems with recommending that everyone who is able to edit a file go ahead and do the fix below. Obviously, take a backup first and know how to restore it if you're not confident.* If you open the file at `wp-content/themes/genesis/lib/structure/comments.php` and look at line 294, then you'll see this: 
    $author = sprintf( '<a href="%s" %s>%s</a>', esc_url( $url ), esc_attr( 'comment-author-link' ), $author );
     Let me write that another way to make it clearer to reference: 

    $author = sprintf(
        '<a href="%s" %s>%s</a>',
        esc_url( $url ),
        esc_attr( 'comment-author-link' ),
        $author
    );
     What we have here is a variable, 

`$author`, being assigned a value from a `sprintf()` call. That call might look odd if you've not seen it before. For the simple case here, take the `%s` in the first argument (second line) as placeholders which are replaced with the corresponding values in the next three lines. The important bit here is the second `%s`, that with the current code is replaced with `esc_attr()`. The `esc_attr()` is a WordPress function that escapes attribute *values* (the bit inside the double quotes). The `esc_url()` function is a special case that handles attribute values which are URLs, so you can see how it replaces the first `%s` correctly. However, we don't want to put an *attribute value* in that second placeholder - we want a list of all the attributes and their values that have been filtered in! Remember that `genesis_attr()` function I mentioned? That takes two arguments, the first of which is a string that provides the context. The second argument is optional and not important here. What we really wanted then was: 
    $author = sprintf(
        '<a href="%s" %s>%s</a>',
        esc_url( $url ),
        genesis_attr( 'comment-author-link' ), // This line changed
        $author
    );
     or in the original format: 

    $author = sprintf( '<a href="%s" %s>%s</a>', esc_url( $url ), genesis_attr( 'comment-author-link' ), $author );
     That will replace the second 

`%s` placeholder with a list of all the attributes and their values (already escaped) for the `comment-author-link` context. Elsewhere in Genesis (`lib/functions/markup.php:619` if you're curious) is a function which uses this context to filter in our `rel` and `itemprop` attributes. 
## Conclusion If you're competent in editing code, I'd say go ahead and make this comment author link change to your copy of Genesis. While it's not usually advised to edit Genesis files, in this case, it's applying the same change as will likely be in the next version of Genesis anyway, so your change will be overwritten with the correct (and likely same) change anyway. If you're using a custom callback for comments (advanced customisation), then you may not be affected by this bug. If your custom callback started off as a copy of 

`genesis_html5_comment_callback()`, then you probably are. 
* * *

## TL;DR Fix {#tldrfix}

*At this moment in time, it's not actually been confirmed as a bug and a bug fix by Copyblogger, but it's so clearcut I have no problems with recommending that everyone who is able to edit a file go ahead and do the fix below. Obviously, take a backup first and know how to restore it if you're not confident.* In `lib/structure/comments.php`, line 294, replace: `$author = sprintf( '<a href="%s" %s>%s</a>', esc_url( $url ), esc_attr( 'comment-author-link' ), $author );` with `$author = sprintf( '<a href="%s" %s>%s</a>', esc_url( $url ), genesis_attr( 'comment-author-link' ), $author );`

 [1]: #tldrfix
 [2]: https://en.wikipedia.org/wiki/Nofollow
 [3]: https://googleblog.blogspot.co.uk/2005/01/preventing-comment-spam.html
 [4]: https://schema.org/Person
 [5]: https://gamajo.com/changes