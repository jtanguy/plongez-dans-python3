What's New In Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#whats-new) ‣

What’s New In “Dive Into Python 3”
==================================

> ❝ Isn’t this where we came in? ❞\
> — Pink Floyd, The Wall

 

*a.k.a.* “the minus level” {#divingin}
--------------------------

Are you already a Python programmer? Did you read the original “[Dive
Into Python](http://diveintopython.org/)”? Did you buy it on paper? (If
so, thanks!) Are you ready to take the plunge into Python 3? … If so,
read on. (If none of that is true, you’d be better off [starting at the
beginning](installing-python.html).)

Python 3 comes with a script called `2to3`. Learn it. Love it. Use it.
[Porting Code to Python 3 with
`2to3`](porting-code-to-python-3-with-2to3.html) is a reference of all
the things that the `2to3` tool can fix automatically. Since a lot of
those things are syntax changes, it’s a good starting point to learn
about a lot of the syntax changes in Python 3. (`print` is now a
function, `` `x` `` doesn’t work, *&*c.)

[Case Study: Porting `chardet` to Python
3](case-study-porting-chardet-to-python-3.html) documents my (ultimately
successful) effort to port a non-trivial library from Python 2 to Python
3. It may help you; it may not. There’s a fairly steep learning curve,
since you need to kind of understand the library first, so you can
understand why it broke and how I fixed it. A lot of the breakage
centers around strings. Speaking of which…

Strings. Whew. Where to start. Python 2 had “strings” and “Unicode
strings.” Python 3 has “bytes” and “strings.” That is, all strings are
now Unicode strings, and if you want to deal with a bag of bytes, you
use the new `bytes` type. Python 3 will *never* implicitly convert
between strings and bytes, so if you’re not sure which one you have at
any given moment, your code will almost certainly break. Read [the
Strings chapter](strings.html) for more details.

Bytes vs. strings comes up again and again throughout the book.

-   In [Files](files.html), you’ll learn the difference between reading
    files in “binary” and “text” mode. Reading (and writing!) files in
    text mode requires an `encoding` parameter. Some text file methods
    count characters, but other methods count bytes. If your code
    assumes that one character == one byte, it *will* break on
    multi-byte characters.
-   In [HTTP Web Services](http-web-services.html), the `httplib2`
    module fetches headers and data over HTTP. HTTP headers are returned
    as strings, but the HTTP body is returned as bytes.
-   In [Serializing Python Objects](serializing.html), you’ll learn why
    the `pickle` module in Python 3 defines a new data format that is
    backwardly incompatible with Python 2. (Hint: it’s because of bytes
    and strings.) Also, Python 3 supports serializing objects to and
    from JSON, which doesn’t even have a `bytes` type. I’ll show you how
    to hack around that.
-   In [Case study: porting `chardet` to Python
    3](case-study-porting-chardet-to-python-3.html), it’s just a bloody
    mess of bytes and strings everywhere.

Even if you don’t care about Unicode (oh but you will), you’ll want to
read about [string formatting in Python
3](strings.html#formatting-strings), which is completely different from
Python 2.

Iterators are everywhere in Python 3, and I understand them a lot better
than I did five years ago when I wrote “Dive Into Python”. You need to
understand them too, because lots of functions that used to return lists
in Python 2 will now return iterators in Python 3. At a minimum, you
should read [the second half of the Iterators
chapter](iterators.html#a-fibonacci-iterator) and [the second half of
the Advanced Iterators
chapter](advanced-iterators.html#generator-expressions).

By popular request, I’ve added an appendix on [Special Method
Names](special-method-names.html), which is kind of like [the Python
docs “Data Model”
chapter](http://www.python.org/doc/3.1/reference/datamodel.html#special-method-names)
but with more snark.

When I was writing “Dive Into Python”, all of the available XML
libraries sucked. Then Fredrik Lundh wrote
[ElementTree](http://effbot.org/zone/element-index.htm), which doesn’t
suck at all. The Python gods wisely [incorporated ElementTree into the
standard
library](http://docs.python.org/3.1/library/xml.etree.elementtree.html),
and now it forms the basis for [my new XML chapter](xml.html). The old
ways of parsing XML are still around, but you should avoid them, because
they suck!

Also new in Python — not in the language but in the community — is the
emergence of code repositories like [The Python Package
Index](http://pypi.python.org/) (PyPI). Python comes with utilities to
package your code in standard formats and distribute those packages on
PyPI. Read [Packaging Python Libraries](packaging.html) for details.

© 2001–11 [Mark Pilgrim](about.html)
