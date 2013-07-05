Colophon - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html) ‣

Colophon
========

> ❝ *Je n’ai fait celle-ci plus longue que parce que je n’ai pas eu le
> loisir de la faire plus courte.*\
> (I would have written a shorter letter, but I did not have the time.)
> ❞\
> — [Blaise Pascal](http://en.wikiquote.org/wiki/Blaise_Pascal)

 

Diving In {#divingin}
---------

This book, like all books, was a labor of love. Oh sure, I got paid the
medium-sized bucks for it, but nobody writes technical books for the
money. And since this book is available on the web as well as on paper,
I spent a lot of time fiddling with webby stuff when I should have been
writing.

![[typewriter]](i/openclipart.org_media_files_johnny_automatic_5261.png)

The online edition loads as efficiently as possible. Efficiency never
happens by accident; I spent many hours making it so. Perhaps too many
hours. Yes, almost certainly too many hours. Never underestimate the
depths to which a procrastinating writer will sink.

I won’t bore you with all the details. Wait, yes — I will bore you with
all the details. But here’s the short version.

1.  HTML is minimized, then served
    [compressed](http://httpd.apache.org/docs/trunk/mod/mod_deflate.html).
2.  Scripts and stylesheets are minimized by [YUI
    Compressor](http://developer.yahoo.com/yui/compressor/) (and also
    served compressed).
3.  Scripts are combined to reduce HTTP requests.
4.  Stylesheets are combined and inlined to reduce HTTP requests.
5.  Unused CSS selectors and properties are [removed on a page-by-page
    basis](http://hg.diveintopython3.org/file/default/util/lesscss.py)
    with a little help from [pyquery](http://pyquery.org/).
6.  HTTP caching and other server-side options are optimized based on
    advice from [YSlow](http://developer.yahoo.com/yslow/) and [Page
    Speed](http://code.google.com/speed/page-speed/).
7.  Pages use [Unicode
    characters](http://www.alanwood.net/unicode/unicode_samples.html) in
    place of images wherever possible.
8.  Images are optimized with
    [OptiPNG](http://optipng.sourceforge.net/).
9.  The entire book was [lovingly hand-authored in HTML
    5](http://diveintomark.org/archives/2009/03/27/dive-into-history-2009-edition)
    to avoid markup cruft.

⁂

Typography
----------

vertical rhythm, best available ampersand, curly quotes/apostrophes,
other stuff from webtypography.net

⁂

Graphics
--------

Unicode, callouts, font-family issues on Windows

⁂

Performance
-----------

"Dive Into History 2009 edition", minimizing CSS + JS + HTML, inline
CSS, optimizing images

⁂

Fun stuff {#fun}
---------

Quotes, constrained writing(?), PapayaWhip

⁂

Further Reading {#furtherreading}
---------------

-   [The Elements of Typographic Style Applied to the
    Web](http://webtypography.net/toc/)
-   [Setting Type on the Web to a Baseline
    Grid](http://www.alistapart.com/articles/settingtypeontheweb)
-   [Compose to a Vertical
    Rhythm](http://24ways.org/2006/compose-to-a-vertical-rhythm)
-   [Use the Best Available
    Ampersand](http://simplebits.com/notebook/2008/08/14/ampersands.html)
-   [Unicode Support in HTML, Fonts, and Web
    Browsers](http://alanwood.net/unicode/)
-   [YSlow](http://developer.yahoo.com/yslow/) for
    [Firebug](http://getfirebug.com/)
-   [Best Practices for Speeding Up Your Web
    Site](http://developer.yahoo.com/performance/rules.html)
-   [14 Rules for Faster-Loading Web
    Sites](http://stevesouders.com/hpws/rules.php)
-   [YUI Compressor](http://developer.yahoo.com/yui/compressor/)
-   [Google Page Speed](http://code.google.com/speed/page-speed/)
-   [Using Google Page
    Speed](http://code.google.com/speed/page-speed/docs/using.html)
-   [OptiPNG](http://optipng.sourceforge.net/)

© 2001–11 [Mark Pilgrim](about.html)
