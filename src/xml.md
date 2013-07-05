XML - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#xml) ‣

Difficulty level: ♦♦♦♦♢

XML
===

> ❝ In the archonship of Aristaechmus, Draco enacted his ordinances. ❞\
> —
> [Aristotle](http://www.perseus.tufts.edu/cgi-bin/ptext?doc=Perseus:text:1999.01.0046;query=chapter%3D%235;layout=;loc=3.1)

 

Diving In {#divingin}
---------

Nearly all the chapters in this book revolve around a piece of sample
code. But XML isn’t about code; it’s about data. One common use of XML
is “syndication feeds” that list the latest articles on a blog, forum,
or other frequently-updated website. Most popular blogging software can
produce a feed and update it whenever new articles, discussion threads,
or blog posts are published. You can follow a blog by “subscribing” to
its feed, and you can follow multiple blogs with a dedicated “[feed
aggregator](http://en.wikipedia.org/wiki/List_of_feed_aggregators)” like
[Google Reader](http://www.google.com/reader/).

Here, then, is the XML data we’ll be working with in this chapter. It’s
a feed — specifically, an [Atom syndication
feed](http://atompub.org/rfc4287.html).

[[download `feed.xml`](examples/feed.xml)]

~~~~ {.pp}
<?xml version='1.0' encoding='utf-8'?>
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'>
  <title>dive into mark</title>
  <subtitle>currently between addictions</subtitle>
  <id>tag:diveintomark.org,2001-07-29:/</id>
  <updated>2009-03-27T21:56:07Z</updated>
  <link rel='alternate' type='text/html' href='http://diveintomark.org/'/>
  <link rel='self' type='application/atom+xml' href='http://diveintomark.org/feed/'/>
  <entry>
    <author>
      <name>Mark</name>
      <uri>http://diveintomark.org/</uri>
    </author>
    <title>Dive into history, 2009 edition</title>
    <link rel='alternate' type='text/html'
      href='http://diveintomark.org/archives/2009/03/27/dive-into-history-2009-edition'/>
    <id>tag:diveintomark.org,2009-03-27:/archives/20090327172042</id>
    <updated>2009-03-27T21:56:07Z</updated>
    <published>2009-03-27T17:20:42Z</published>
    <category scheme='http://diveintomark.org' term='diveintopython'/>
    <category scheme='http://diveintomark.org' term='docbook'/>
    <category scheme='http://diveintomark.org' term='html'/>
  <summary type='html'>Putting an entire chapter on one page sounds
    bloated, but consider this &amp;mdash; my longest chapter so far
    would be 75 printed pages, and it loads in under 5 seconds&amp;hellip;
    On dialup.</summary>
  </entry>
  <entry>
    <author>
      <name>Mark</name>
      <uri>http://diveintomark.org/</uri>
    </author>
    <title>Accessibility is a harsh mistress</title>
    <link rel='alternate' type='text/html'
      href='http://diveintomark.org/archives/2009/03/21/accessibility-is-a-harsh-mistress'/>
    <id>tag:diveintomark.org,2009-03-21:/archives/20090321200928</id>
    <updated>2009-03-22T01:05:37Z</updated>
    <published>2009-03-21T20:09:28Z</published>
    <category scheme='http://diveintomark.org' term='accessibility'/>
    <summary type='html'>The accessibility orthodoxy does not permit people to
      question the value of features that are rarely useful and rarely used.</summary>
  </entry>
  <entry>
    <author>
      <name>Mark</name>
    </author>
    <title>A gentle introduction to video encoding, part 1: container formats</title>
    <link rel='alternate' type='text/html'
      href='http://diveintomark.org/archives/2008/12/18/give-part-1-container-formats'/>
    <id>tag:diveintomark.org,2008-12-18:/archives/20081218155422</id>
    <updated>2009-01-11T19:39:22Z</updated>
    <published>2008-12-18T15:54:22Z</published>
    <category scheme='http://diveintomark.org' term='asf'/>
    <category scheme='http://diveintomark.org' term='avi'/>
    <category scheme='http://diveintomark.org' term='encoding'/>
    <category scheme='http://diveintomark.org' term='flv'/>
    <category scheme='http://diveintomark.org' term='GIVE'/>
    <category scheme='http://diveintomark.org' term='mp4'/>
    <category scheme='http://diveintomark.org' term='ogg'/>
    <category scheme='http://diveintomark.org' term='video'/>
    <summary type='html'>These notes will eventually become part of a
      tech talk on video encoding.</summary>
  </entry>
</feed>
~~~~

⁂

A 5-Minute Crash Course in XML {#xml-intro}
------------------------------

If you already know about XML, you can skip this section.

XML is a generalized way of describing hierarchical structured data. An
XML *document* contains one or more *elements*, which are delimited by
*start and end tags*. This is a complete (albeit boring) XML document:

~~~~ {.nd .pp}
<foo>   ①
</foo>  ②
~~~~

1.  This is the *start tag* of the `foo` element.
2.  This is the matching *end tag* of the `foo` element. Like balancing
    parentheses in writing or mathematics or code, every start tag must
    be *closed* (matched) by a corresponding end tag.

Elements can be *nested* to any depth. An element `bar` inside an
element `foo` is said to be a *subelement* or *child* of `foo`.

~~~~ {.nd .pp}
<foo>
  <bar></bar>
</foo>
~~~~

The first element in every XML document is called the *root element*. An
XML document can only have one root element. The following is **not an
XML document**, because it has two root elements:

~~~~ {.nd .pp}
<foo></foo>
<bar></bar>
~~~~

Elements can have *attributes*, which are name-value pairs. Attributes
are listed within the start tag of an element and separated by
whitespace. *Attribute names* can not be repeated within an element.
*Attribute values* must be quoted. You may use either single or double
quotes.

~~~~ {.nd .pp}
<foo lang='en'>                          ①
  <bar id='papayawhip' lang="fr"></bar>  ②
</foo>
~~~~

1.  The `foo` element has one attribute, named `lang`. The value of its
    `lang` attribute is `en`.
2.  The `bar` element has two attributes, named `id` and `lang`. The
    value of its `lang` attribute is `fr`. This doesn’t conflict with
    the `foo` element in any way. Each element has its own set of
    attributes.

If an element has more than one attribute, the ordering of the
attributes is not significant. An element’s attributes form an unordered
set of keys and values, like a Python dictionary. There is no limit to
the number of attributes you can define on each element.

Elements can have *text content*.

~~~~ {.nd .pp}
<foo lang='en'>
  <bar lang='fr'>PapayaWhip</bar>
</foo>
~~~~

Elements that contain no text and no children are *empty*.

~~~~ {.nd .pp}
<foo></foo>
~~~~

There is a shorthand for writing empty elements. By putting a `/`
character in the start tag, you can skip the end tag altogther. The XML
document in the previous example could be written like this instead:

~~~~ {.nd .pp}
<foo/>
~~~~

Like Python functions can be declared in different *modules*, XML
elements can be declared in different *namespaces*. Namespaces usually
look like URLs. You use an `xmlns` declaration to define a *default
namespace*. A namespace declaration looks similar to an attribute, but
it has a different purpose.

~~~~ {.nd .pp}
<feed xmlns='http://www.w3.org/2005/Atom'>  ①
  <title>dive into mark</title>             ②
</feed>
~~~~

1.  The `feed` element is in the `http://www.w3.org/2005/Atom`
    namespace.
2.  The `title` element is also in the `http://www.w3.org/2005/Atom`
    namespace. The namespace declaration affects the element where it’s
    declared, plus all child elements.

You can also use an `xmlns:prefix` declaration to define a namespace and
associate it with a *prefix*. Then each element in that namespace must
be explicitly declared with the prefix.

~~~~ {.nd .pp}
<atom:feed xmlns:atom='http://www.w3.org/2005/Atom'>  ①
  <atom:title>dive into mark</atom:title>             ②
</atom:feed>
~~~~

1.  The `feed` element is in the `http://www.w3.org/2005/Atom`
    namespace.
2.  The `title` element is also in the `http://www.w3.org/2005/Atom`
    namespace.

As far as an XML parser is concerned, the previous two XML documents are
*identical*. Namespace + element name = XML identity. Prefixes only
exist to refer to namespaces, so the actual prefix name (`atom:`) is
irrelevant. The namespaces match, the element names match, the
attributes (or lack of attributes) match, and each element’s text
content matches, therefore the XML documents are the same.

Finally, XML documents can contain [character encoding
information](strings.html#one-ring-to-rule-them-all) on the first line,
before the root element. (If you’re curious how a document can contain
information which needs to be known before the document can be parsed,
[Section F of the XML
specification](http://www.w3.org/TR/REC-xml/#sec-guessing-no-ext-info)
details how to resolve this Catch-22.)

~~~~ {.nd .pp}
<?xml version='1.0' encoding='utf-8'?>
~~~~

And now you know just enough XML to be dangerous!

⁂

The Structure Of An Atom Feed {#xml-structure}
-----------------------------

Think of a weblog, or in fact any website with frequently updated
content, like [CNN.com](http://www.cnn.com/). The site itself has a
title (“CNN.com”), a subtitle (“Breaking News, U.S., World, Weather,
Entertainment *&* Video News”), a last-updated date (“updated 12:43 p.m.
EDT, Sat May 16, 2009”), and a list of articles posted at different
times. Each article also has a title, a first-published date (and maybe
also a last-updated date, if they published a correction or fixed a
typo), and a unique URL.

The Atom syndication format is designed to capture all of this
information in a standard format. My weblog and CNN.com are wildly
different in design, scope, and audience, but they both have the same
basic structure. CNN.com has a title; my blog has a title. CNN.com
publishes articles; I publish articles.

At the top level is the *root element*, which every Atom feed shares:
the `feed` element in the `http://www.w3.org/2005/Atom` namespace.

~~~~ {.pp}
<feed xmlns='http://www.w3.org/2005/Atom'  ①
      xml:lang='en'>                       ②
~~~~

1.  `http://www.w3.org/2005/Atom` is the Atom namespace.
2.  Any element can contain an `xml:lang` attribute, which declares the
    language of the element and its children. In this case, the
    `xml:lang` attribute is declared once on the root element, which
    means the entire feed is in English.

An Atom feed contains several pieces of information about the feed
itself. These are declared as children of the root-level `feed` element.

~~~~ {.pp}
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'>
  <title>dive into mark</title>                                             ①
  <subtitle>currently between addictions</subtitle>                         ②
  <id>tag:diveintomark.org,2001-07-29:/</id>                                ③
  <updated>2009-03-27T21:56:07Z</updated>                                   ④
  <link rel='alternate' type='text/html' href='http://diveintomark.org/'/>  ⑤
~~~~

1.  The title of this feed is `dive into mark`.
2.  The subtitle of this feed is `currently between addictions`.
3.  Every feed needs a globally unique identifier. See [RFC
    4151](http://www.ietf.org/rfc/rfc4151.txt) for how to create one.
4.  This feed was last updated on March 27, 2009, at 21:56 GMT. This is
    usually equivalent to the last-modified date of the most recent
    article.
5.  Now things start to get interesting. This `link` element has no text
    content, but it has three attributes: `rel`, `type`, and `href`. The
    `rel` value tells you what kind of link this is; `rel='alternate'`
    means that this is a link to an alternate representation of this
    feed. The `type='text/html'` attribute means that this is a link to
    an HTML page. And the link target is given in the `href` attribute.

Now we know that this is a feed for a site named “dive into mark“ which
is available at [`http://diveintomark.org/`](http://diveintomark.org/)
and was last updated on March 27, 2009.

> ☞Although the order of elements can be relevant in some XML documents,
> it is not relevant in an Atom feed.

After the feed-level metadata is the list of the most recent articles.
An article looks like this:

~~~~ {.pp}
<entry>
  <author>                                                                 ①
    <name>Mark</name>
    <uri>http://diveintomark.org/</uri>
  </author>
  <title>Dive into history, 2009 edition</title>                           ②
  <link rel='alternate' type='text/html'                                   ③
    href='http://diveintomark.org/archives/2009/03/27/dive-into-history-2009-edition'/>
  <id>tag:diveintomark.org,2009-03-27:/archives/20090327172042</id>        ④
  <updated>2009-03-27T21:56:07Z</updated>                                  ⑤
  <published>2009-03-27T17:20:42Z</published>        
  <category scheme='http://diveintomark.org' term='diveintopython'/>       ⑥
  <category scheme='http://diveintomark.org' term='docbook'/>
  <category scheme='http://diveintomark.org' term='html'/>
  <summary type='html'>Putting an entire chapter on one page sounds        ⑦
    bloated, but consider this &amp;mdash; my longest chapter so far
    would be 75 printed pages, and it loads in under 5 seconds&amp;hellip;
    On dialup.</summary>
</entry>                                                                   ⑧
~~~~

1.  The `author` element tells who wrote this article: some guy named
    Mark, whom you can find loafing at `http://diveintomark.org/`. (This
    is the same as the alternate link in the feed metadata, but it
    doesn’t have to be. Many weblogs have multiple authors, each with
    their own personal website.)
2.  The `title` element gives the title of the article, “Dive into
    history, 2009 edition”.
3.  As with the feed-level alternate link, this `link` element gives the
    address of the HTML version of this article.
4.  Entries, like feeds, need a unique identifier.
5.  Entries have two dates: a first-published date (`published`) and a
    last-modified date (`updated`).
6.  Entries can have an arbitrary number of categories. This article is
    filed under `diveintopython`, `docbook`, and `html`.
7.  The `summary` element gives a brief summary of the article. (There
    is also a `content` element, not shown here, if you want to include
    the complete article text in your feed.) This `summary` element has
    the Atom-specific `type='html'` attribute, which specifies that this
    summary is a snippet of HTML, not plain text. This is important,
    since it has HTML-specific entities in it (`&mdash;` and `&hellip;`)
    which should be rendered as “—” and “…” rather than displayed
    directly.
8.  Finally, the end tag for the `entry` element, signaling the end of
    the metadata for this article.

⁂

Parsing XML {#xml-parse}
-----------

Python can parse XML documents in several ways. It has traditional
[DOM](http://en.wikipedia.org/wiki/XML#DOM) and
[SAX](http://en.wikipedia.org/wiki/Simple_API_for_XML) parsers, but I
will focus on a different library called ElementTree.

[[download `feed.xml`](examples/feed.xml)]

~~~~ {.screen}
>>> import xml.etree.ElementTree as etree    ①
>>> tree = etree.parse('examples/feed.xml')  ②
>>> root = tree.getroot()                    ③
>>> root                                     ④
<Element {http://www.w3.org/2005/Atom}feed at cd1eb0>
~~~~

1.  The ElementTree library is part of the Python standard library, in
    `xml.etree.ElementTree`.
2.  The primary entry point for the ElementTree library is the `parse()`
    function, which can take a filename or a [file-like
    object](files.html#file-like-objects). This function parses the
    entire document at once. If memory is tight, there are ways to
    [parse an XML document incrementally
    instead](http://effbot.org/zone/element-iterparse.htm).
3.  The `parse()` function returns an object which represents the entire
    document. This is *not* the root element. To get a reference to the
    root element, call the `getroot()` method.
4.  As expected, the root element is the `feed` element in the
    `http://www.w3.org/2005/Atom` namespace. The string representation
    of this object reinforces an important point: an XML element is a
    combination of its namespace and its tag name (also called the
    *local name*). Every element in this document is in the Atom
    namespace, so the root element is represented as
    `{http://www.w3.org/2005/Atom}feed`.

> ☞ElementTree represents XML elements as `{namespace}localname`. You’ll
> see and use this format in multiple places in the ElementTree API.

### Elements Are Lists {#xml-elements}

In the ElementTree API, an element acts like a list. The items of the
list are the element’s children.

~~~~ {.screen}
# continued from the previous example
>>> root.tag                        ①
'{http://www.w3.org/2005/Atom}feed'
>>> len(root)                       ②
8
>>> for child in root:              ③
...   print(child)                  ④
... 
<Element {http://www.w3.org/2005/Atom}title at e2b5d0>
<Element {http://www.w3.org/2005/Atom}subtitle at e2b4e0>
<Element {http://www.w3.org/2005/Atom}id at e2b6c0>
<Element {http://www.w3.org/2005/Atom}updated at e2b6f0>
<Element {http://www.w3.org/2005/Atom}link at e2b4b0>
<Element {http://www.w3.org/2005/Atom}entry at e2b720>
<Element {http://www.w3.org/2005/Atom}entry at e2b510>
<Element {http://www.w3.org/2005/Atom}entry at e2b750>
~~~~

1.  Continuing from the previous example, the root element is
    `{http://www.w3.org/2005/Atom}feed`.
2.  The “length” of the root element is the number of child elements.
3.  You can use the element itself as an iterator to loop through all of
    its child elements.
4.  As you can see from the output, there are indeed 8 child elements:
    all of the feed-level metadata (`title`, `subtitle`, `id`,
    `updated`, and `link`) followed by the three `entry` elements.

You may have guessed this already, but I want to point it out
explicitly: the list of child elements only includes *direct* children.
Each of the `entry` elements contain their own children, but those are
not included in the list. They would be included in the list of each
`entry`’s children, but they are not included in the list of the
`feed`’s children. There are ways to find elements no matter how deeply
nested they are; we’ll look at two such ways later in this chapter.

### Attributes Are Dictonaries {#xml-attributes}

XML isn’t just a collection of elements; each element can also have its
own set of attributes. Once you have a reference to a specific element,
you can easily get its attributes as a Python dictionary.

~~~~ {.screen}
# continuing from the previous example
>>> root.attrib                           ①
{'{http://www.w3.org/XML/1998/namespace}lang': 'en'}
>>> root[4]                               ②
<Element {http://www.w3.org/2005/Atom}link at e181b0>
>>> root[4].attrib                        ③
{'href': 'http://diveintomark.org/',
 'type': 'text/html',
 'rel': 'alternate'}
>>> root[3]                               ④
<Element {http://www.w3.org/2005/Atom}updated at e2b4e0>
>>> root[3].attrib                        ⑤
{}
~~~~

1.  The `attrib` property is a dictionary of the element’s attributes.
    The original markup here was
    `<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'>`. The
    `xml:` prefix refers to a built-in namespace that every XML document
    can use without declaring it.
2.  The fifth child — `[4]` in a 0-based list — is the `link` element.
3.  The `link` element has three attributes: `href`, `type`, and `rel`.
4.  The fourth child — `[3]` in a 0-based list — is the `updated`
    element.
5.  The `updated` element has no attributes, so its `.attrib` is just an
    empty dictionary.

⁂

Searching For Nodes Within An XML Document {#xml-find}
------------------------------------------

So far, we’ve worked with this XML document “from the top down,”
starting with the root element, getting its child elements, and so on
throughout the document. But many uses of XML require you to find
specific elements. Etree can do that, too.

~~~~ {.screen}
>>> import xml.etree.ElementTree as etree
>>> tree = etree.parse('examples/feed.xml')
>>> root = tree.getroot()
>>> root.findall('{http://www.w3.org/2005/Atom}entry')    ①
[<Element {http://www.w3.org/2005/Atom}entry at e2b4e0>,
 <Element {http://www.w3.org/2005/Atom}entry at e2b510>,
 <Element {http://www.w3.org/2005/Atom}entry at e2b540>]
>>> root.tag
'{http://www.w3.org/2005/Atom}feed'
>>> root.findall('{http://www.w3.org/2005/Atom}feed')     ②
[]
>>> root.findall('{http://www.w3.org/2005/Atom}author')   ③
[]
~~~~

1.  The `findall()` method finds child elements that match a specific
    query. (More on the query format in a minute.)
2.  Each element — including the root element, but also child
    elements — has a `findall()` method. It finds all matching elements
    among the element’s children. But why aren’t there any results?
    Although it may not be obvious, this particular query only searches
    the element’s children. Since the root `feed` element has no child
    named `feed`, this query returns an empty list.
3.  This result may also surprise you. [There is an `author`
    element](#divingin) in this document; in fact, there are three (one
    in each `entry`). But those `author` elements are not *direct
    children* of the root element; they are “grandchildren” (literally,
    a child element of a child element). If you want to look for
    `author` elements at any nesting level, you can do that, but the
    query format is slightly different.

~~~~ {.screen}
>>> tree.findall('{http://www.w3.org/2005/Atom}entry')    ①
[<Element {http://www.w3.org/2005/Atom}entry at e2b4e0>,
 <Element {http://www.w3.org/2005/Atom}entry at e2b510>,
 <Element {http://www.w3.org/2005/Atom}entry at e2b540>]
>>> tree.findall('{http://www.w3.org/2005/Atom}author')   ②
[]
~~~~

1.  For convenience, the `tree` object (returned from the
    `etree.parse()` function) has several methods that mirror the
    methods on the root element. The results are the same as if you had
    called the `tree.getroot().findall()` method.
2.  Perhaps surprisingly, this query does not find the `author` elements
    in this document. Why not? Because this is just a shortcut for
    `tree.getroot().findall('{http://www.w3.org/2005/Atom}author')`,
    which means “find all the `author` elements that are children of the
    root element.” The `author` elements are not children of the root
    element; they’re children of the `entry` elements. Thus the query
    doesn’t return any matches.

There is also a `find()` method which returns the first matching
element. This is useful for situations where you are only expecting one
match, or if there are multiple matches, you only care about the first
one.

~~~~ {.screen}
>>> entries = tree.findall('{http://www.w3.org/2005/Atom}entry')           ①
>>> len(entries)
3
>>> title_element = entries[0].find('{http://www.w3.org/2005/Atom}title')  ②
>>> title_element.text
'Dive into history, 2009 edition'
>>> foo_element = entries[0].find('{http://www.w3.org/2005/Atom}foo')      ③
>>> foo_element
>>> type(foo_element)
<class 'NoneType'>
~~~~

1.  You saw this in the previous example. It finds all the `atom:entry`
    elements.
2.  The `find()` method takes an ElementTree query and returns the first
    matching element.
3.  There are no elements in this entry named `foo`, so this returns
    `None`.

> ☞There is a “gotcha” with the `find()` method that will eventually
> bite you. In a boolean context, ElementTree element objects will
> evaluate to `False` if they contain no children (*i.e.* if
> `len(element)` is 0). This means that `if element.find('...')` is not
> testing whether the `find()` method found a matching element; it’s
> testing whether that matching element has any child elements! To test
> whether the `find()` method returned an element, use
> `if element.find('...') is not None`.

There *is* a way to search for *descendant* elements, *i.e.* children,
grandchildren, and any element at any nesting level.

~~~~ {.screen}
>>> all_links = tree.findall('//{http://www.w3.org/2005/Atom}link')  ①
>>> all_links
[<Element {http://www.w3.org/2005/Atom}link at e181b0>,
 <Element {http://www.w3.org/2005/Atom}link at e2b570>,
 <Element {http://www.w3.org/2005/Atom}link at e2b480>,
 <Element {http://www.w3.org/2005/Atom}link at e2b5a0>]
>>> all_links[0].attrib                                              ②
{'href': 'http://diveintomark.org/',
 'type': 'text/html',
 'rel': 'alternate'}
>>> all_links[1].attrib                                              ③
{'href': 'http://diveintomark.org/archives/2009/03/27/dive-into-history-2009-edition',
 'type': 'text/html',
 'rel': 'alternate'}
>>> all_links[2].attrib
{'href': 'http://diveintomark.org/archives/2009/03/21/accessibility-is-a-harsh-mistress',
 'type': 'text/html',
 'rel': 'alternate'}
>>> all_links[3].attrib
{'href': 'http://diveintomark.org/archives/2008/12/18/give-part-1-container-formats',
 'type': 'text/html',
 'rel': 'alternate'}
~~~~

1.  This query — `//{http://www.w3.org/2005/Atom}link` — is very similar
    to the previous examples, except for the two slashes at the
    beginning of the query. Those two slashes mean “don’t just look for
    direct children; I want *any* elements, regardless of nesting
    level.” So the result is a list of four `link` elements, not just
    one.
2.  The first result *is* a direct child of the root element. As you can
    see from its attributes, this is the feed-level alternate link that
    points to the HTML version of the website that the feed describes.
3.  The other three results are each entry-level alternate links. Each
    `entry` has a single `link` child element, and because of the double
    slash at the beginning of the query, this query finds all of them.

Overall, ElementTree’s `findall()` method is a very powerful feature,
but the query language can be a bit surprising. It is officially
described as “[limited support for XPath
expressions](http://effbot.org/zone/element-xpath.htm).”
[XPath](http://www.w3.org/TR/xpath) is a W3C standard for querying XML
documents. ElementTree’s query language is similar enough to XPath to do
basic searching, but dissimilar enough that it may annoy you if you
already know XPath. Now let’s look at a third-party XML library that
extends the ElementTree API with full XPath support.

⁂

Going Further With lxml {#xml-lxml}
-----------------------

[`lxml`](http://codespeak.net/lxml/) is an open source third-party
library that builds on the popular [libxml2
parser](http://www.xmlsoft.org/). It provides a 100% compatible
ElementTree API, then extends it with full XPath 1.0 support and a few
other niceties. There are [installers available for
Windows](http://pypi.python.org/pypi/lxml/); Linux users should always
try to use distribution-specific tools like `yum` or `apt-get` to
install precompiled binaries from their repositories. Otherwise you’ll
need to [install `lxml`
manually](http://codespeak.net/lxml/installation.html).

~~~~ {.screen}
>>> from lxml import etree                   ①
>>> tree = etree.parse('examples/feed.xml')  ②
>>> root = tree.getroot()                    ③
>>> root.findall('{http://www.w3.org/2005/Atom}entry')  ④
[<Element {http://www.w3.org/2005/Atom}entry at e2b4e0>,
 <Element {http://www.w3.org/2005/Atom}entry at e2b510>,
 <Element {http://www.w3.org/2005/Atom}entry at e2b540>]
~~~~

1.  Once imported, `lxml` provides the same API as the built-in
    ElementTree library.
2.  `parse()` function: same as ElementTree.
3.  `getroot()` method: also the same.
4.  `findall()` method: exactly the same.

For large XML documents, `lxml` is significantly faster than the
built-in ElementTree library. If you’re only using the ElementTree API
and want to use the fastest available implementation, you can try to
import `lxml` and fall back to the built-in ElementTree.

~~~~ {.nd .pp}
try:
    from lxml import etree
except ImportError:
    import xml.etree.ElementTree as etree
~~~~

But `lxml` is more than just a faster ElementTree. Its `findall()`
method includes support for more complicated expressions.

~~~~ {.screen}
>>> import lxml.etree                                                                   ①
>>> tree = lxml.etree.parse('examples/feed.xml')
>>> tree.findall('//{http://www.w3.org/2005/Atom}*[@href]')                             ②
[<Element {http://www.w3.org/2005/Atom}link at eeb8a0>,
 <Element {http://www.w3.org/2005/Atom}link at eeb990>,
 <Element {http://www.w3.org/2005/Atom}link at eeb960>,
 <Element {http://www.w3.org/2005/Atom}link at eeb9c0>]
>>> tree.findall("//{http://www.w3.org/2005/Atom}*[@href='http://diveintomark.org/']")  ③
[<Element {http://www.w3.org/2005/Atom}link at eeb930>]
>>> NS = '{http://www.w3.org/2005/Atom}'
>>> tree.findall('//{NS}author[{NS}uri]'.format(NS=NS))                                 ④
[<Element {http://www.w3.org/2005/Atom}author at eeba80>,
 <Element {http://www.w3.org/2005/Atom}author at eebba0>]
~~~~

1.  In this example, I’m going to `import lxml.etree` (instead of, say,
    `from lxml import etree`), to emphasize that these features are
    specific to `lxml`.
2.  This query finds all elements in the Atom namespace, anywhere in the
    document, that have an `href` attribute. The `//` at the beginning
    of the query means “elements anywhere (not just as children of the
    root element).” `{http://www.w3.org/2005/Atom}` means “only elements
    in the Atom namespace.” `*` means “elements with any local name.”
    And `[@href]` means “has an `href` attribute.”
3.  The query finds all Atom elements with an `href` whose value is
    `http://diveintomark.org/`.
4.  After doing some quick [string
    formatting](strings.html#formatting-strings) (because otherwise
    these compound queries get ridiculously long), this query searches
    for Atom `author` elements that have an Atom `uri` element as a
    child. This only returns two `author` elements, the ones in the
    first and second `entry`. The `author` in the last `entry` contains
    only a `name`, not a `uri`.

Not enough for you? `lxml` also integrates support for arbitrary XPath
1.0 expressions. I’m not going to go into depth about XPath syntax; that
could be a whole book unto itself! But I will show you how it integrates
into `lxml`.

~~~~ {.screen}
>>> import lxml.etree
>>> tree = lxml.etree.parse('examples/feed.xml')
>>> NSMAP = {'atom': 'http://www.w3.org/2005/Atom'}                    ①
>>> entries = tree.xpath("//atom:category[@term='accessibility']/..",  ②
...     namespaces=NSMAP)
>>> entries                                                            ③
[<Element {http://www.w3.org/2005/Atom}entry at e2b630>]
>>> entry = entries[0]
>>> entry.xpath('./atom:title/text()', namespaces=NSMAP)               ④
['Accessibility is a harsh mistress']
~~~~

1.  To perform XPath queries on namespaced elements, you need to define
    a namespace prefix mapping. This is just a Python dictionary.
2.  Here is an XPath query. The XPath expression searches for `category`
    elements (in the Atom namespace) that contain a `term` attribute
    with the value `accessibility`. But that’s not actually the query
    result. Look at the very end of the query string; did you notice the
    `/..` bit? That means “and then return the parent element of the
    `category` element you just found.” So this single XPath query will
    find all entries with a child element of
    `<category term='accessibility'>`.
3.  The `xpath()` function returns a list of ElementTree objects. In
    this document, there is only one entry with a `category` whose
    `term` is `accessibility`.
4.  XPath expressions don’t always return a list of elements.
    Technically, the DOM of a parsed XML document doesn’t contain
    elements; it contains *nodes*. Depending on their type, nodes can be
    elements, attributes, or even text content. The result of an XPath
    query is a list of nodes. This query returns a list of text nodes:
    the text content (`text()`) of the `title` element (`atom:title`)
    that is a child of the current element (`./`).

⁂

Generating XML {#xml-generate}
--------------

Python’s support for XML is not limited to parsing existing documents.
You can also create XML documents from scratch.

~~~~ {.screen}
>>> import xml.etree.ElementTree as etree
>>> new_feed = etree.Element('{http://www.w3.org/2005/Atom}feed',     ①
...     attrib={'{http://www.w3.org/XML/1998/namespace}lang': 'en'})  ②
>>> print(etree.tostring(new_feed))                                   ③
<ns0:feed xmlns:ns0='http://www.w3.org/2005/Atom' xml:lang='en'/>
~~~~

1.  To create a new element, instantiate the `Element` class. You pass
    the element name (namespace + local name) as the first argument.
    This statement creates a `feed` element in the Atom namespace. This
    will be our new document’s root element.
2.  To add attributes to the newly created element, pass a dictionary of
    attribute names and values in the attrib argument. Note that the
    attribute name should be in the standard ElementTree format,
    `{namespace}localname`.
3.  At any time, you can serialize any element (and its children) with
    the ElementTree `tostring()` function.

Was that serialization surprising to you? The way ElementTree serializes
namespaced XML elements is technically accurate but not optimal. The
sample XML document at the beginning of this chapter defined a *default
namespace* (`xmlns='http://www.w3.org/2005/Atom'`). Defining a default
namespace is useful for documents — like Atom feeds — where every
element is in the same namespace, because you can declare the namespace
once and declare each element with just its local name (`<feed>`,
`<link>`, `<entry>`). There is no need to use any prefixes unless you
want to declare elements from another namespace.

An XML parser won’t “see” any difference between an XML document with a
default namespace and an XML document with a prefixed namespace. The
resulting DOM of this serialization:

~~~~ {.nd .pp}
<ns0:feed xmlns:ns0='http://www.w3.org/2005/Atom' xml:lang='en'/>
~~~~

is identical to the DOM of this serialization:

~~~~ {.nd .pp}
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'/>
~~~~

The only practical difference is that the second serialization is
several characters shorter. If we were to recast our entire sample feed
with a `ns0:` prefix in every start and end tag, it would add 4
characters per start tag × 79 tags + 4 characters for the namespace
declaration itself, for a total of 320 characters. Assuming [UTF-8
encoding](strings.html#byte-arrays), that’s 320 extra bytes. (After
gzipping, the difference drops to 21 bytes, but still, 21 bytes is 21
bytes.) Maybe that doesn’t matter to you, but for something like an Atom
feed, which may be downloaded several thousand times whenever it
changes, saving a few bytes per request can quickly add up.

The built-in ElementTree library does not offer this fine-grained
control over serializing namespaced elements, but `lxml` does.

~~~~ {.screen}
>>> import lxml.etree
>>> NSMAP = {None: 'http://www.w3.org/2005/Atom'}                     ①
>>> new_feed = lxml.etree.Element('feed', nsmap=NSMAP)                ②
>>> print(lxml.etree.tounicode(new_feed))                             ③
<feed xmlns='http://www.w3.org/2005/Atom'/>
>>> new_feed.set('{http://www.w3.org/XML/1998/namespace}lang', 'en')  ④
>>> print(lxml.etree.tounicode(new_feed))
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'/>
~~~~

1.  To start, define a namespace mapping as a dictionary. Dictionary
    values are namespaces; dictionary keys are the desired prefix. Using
    `None` as a prefix effectively declares a default namespace.
2.  Now you can pass the `lxml`-specific nsmap argument when you create
    an element, and `lxml` will respect the namespace prefixes you’ve
    defined.
3.  As expected, this serialization defines the Atom namespace as the
    default namespace and declares the `feed` element without a
    namespace prefix.
4.  Oops, we forgot to add the `xml:lang` attribute. You can always add
    attributes to any element with the `set()` method. It takes two
    arguments: the attribute name in standard ElementTree format, then
    the attribute value. (This method is not `lxml`-specific. The only
    `lxml`-specific part of this example was the nsmap argument to
    control the namespace prefixes in the serialized output.)

Are XML documents limited to one element per document? No, of course
not. You can easily create child elements, too.

~~~~ {.screen}
>>> title = lxml.etree.SubElement(new_feed, 'title',          ①
...     attrib={'type':'html'})                               ②
>>> print(lxml.etree.tounicode(new_feed))                     ③
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'><title type='html'/></feed>
>>> title.text = 'dive into &hellip;'                         ④
>>> print(lxml.etree.tounicode(new_feed))                     ⑤
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'><title type='html'>dive into &amp;hellip;</title></feed>
>>> print(lxml.etree.tounicode(new_feed, pretty_print=True))  ⑥
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'>
<title type='html'>dive into&amp;hellip;</title>
</feed>
~~~~

1.  To create a child element of an existing element, instantiate the
    `SubElement` class. The only required arguments are the parent
    element (new\_feed in this case) and the new element’s name. Since
    this child element will inherit the namespace mapping of its parent,
    there is no need to redeclare the namespace or prefix here.
2.  You can also pass in an attribute dictionary. Keys are attribute
    names; values are attribute values.
3.  As expected, the new `title` element was created in the Atom
    namespace, and it was inserted as a child of the `feed` element.
    Since the `title` element has no text content and no children of its
    own, `lxml` serializes it as an empty element (with the `/>`
    shortcut).
4.  To set the text content of an element, simply set its `.text`
    property.
5.  Now the `title` element is serialized with its text content. Any
    text content that contains less-than signs or ampersands needs to be
    escaped when serialized. `lxml` handles this escaping automatically.
6.  You can also apply “pretty printing” to the serialization, which
    inserts line breaks after end tags, and after start tags of elements
    that contain child elements but no text content. In technical terms,
    `lxml` adds “insignificant whitespace” to make the output more
    readable.

> ☞You might also want to check out
> [xmlwitch](http://github.com/galvez/xmlwitch/tree/master), another
> third-party library for generating XML. It makes extensive use of [the
> `with` statement](special-method-names.html#context-managers) to make
> XML generation code more readable.

⁂

Parsing Broken XML {#xml-custom-parser}
------------------

The XML specification mandates that all conforming XML parsers employ
“draconian error handling.” That is, they must halt and catch fire as
soon as they detect any sort of wellformedness error in the XML
document. Wellformedness errors include mismatched start and end tags,
undefined entities, illegal Unicode characters, and a number of other
esoteric rules. This is in stark contrast to other common formats like
HTML — your browser doesn’t stop rendering a web page if you forget to
close an HTML tag or escape an ampersand in an attribute value. (It is a
common misconception that HTML has no defined error handling. [HTML
error
handling](http://www.whatwg.org/specs/web-apps/current-work/multipage/syntax.html#parsing)
is actually quite well-defined, but it’s significantly more complicated
than “halt and catch fire on first error.”)

Some people (myself included) believe that it was a mistake for the
inventors of XML to mandate draconian error handling. Don’t get me
wrong; I can certainly see the allure of simplifying the error handling
rules. But in practice, the concept of “wellformedness” is trickier than
it sounds, especially for XML documents (like Atom feeds) that are
published on the web and served over HTTP. Despite the maturity of XML,
which standardized on draconian error handling in 1997, surveys
continually show a significant fraction of Atom feeds on the web are
plagued with wellformedness errors.

So, I have both theoretical and practical reasons to parse XML documents
“at any cost,” that is, *not* to halt and catch fire at the first
wellformedness error. If you find yourself wanting to do this too,
`lxml` can help.

Here is a fragment of a broken XML document. I’ve highlighted the
wellformedness error.

~~~~ {.nd .pp}
<?xml version='1.0' encoding='utf-8'?>
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'>
  <title>dive into &hellip;</title>
...
</feed>
~~~~

That’s an error, because the `&hellip;` entity is not defined in XML.
(It is defined in HTML.) If you try to parse this broken feed with the
default settings, `lxml` will choke on the undefined entity.

~~~~ {.nd .screen}
>>> import lxml.etree
>>> tree = lxml.etree.parse('examples/feed-broken.xml')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "lxml.etree.pyx", line 2693, in lxml.etree.parse (src/lxml/lxml.etree.c:52591)
  File "parser.pxi", line 1478, in lxml.etree._parseDocument (src/lxml/lxml.etree.c:75665)
  File "parser.pxi", line 1507, in lxml.etree._parseDocumentFromURL (src/lxml/lxml.etree.c:75993)
  File "parser.pxi", line 1407, in lxml.etree._parseDocFromFile (src/lxml/lxml.etree.c:75002)
  File "parser.pxi", line 965, in lxml.etree._BaseParser._parseDocFromFile (src/lxml/lxml.etree.c:72023)
  File "parser.pxi", line 539, in lxml.etree._ParserContext._handleParseResultDoc (src/lxml/lxml.etree.c:67830)
  File "parser.pxi", line 625, in lxml.etree._handleParseResult (src/lxml/lxml.etree.c:68877)
  File "parser.pxi", line 565, in lxml.etree._raiseParseError (src/lxml/lxml.etree.c:68125)
lxml.etree.XMLSyntaxError: Entity 'hellip' not defined, line 3, column 28
~~~~

To parse this broken XML document, despite its wellformedness error, you
need to create a custom XML parser.

~~~~ {.screen}
>>> parser = lxml.etree.XMLParser(recover=True)                  ①
>>> tree = lxml.etree.parse('examples/feed-broken.xml', parser)  ②
>>> parser.error_log                                             ③
examples/feed-broken.xml:3:28:FATAL:PARSER:ERR_UNDECLARED_ENTITY: Entity 'hellip' not defined
>>> tree.findall('{http://www.w3.org/2005/Atom}title')
[<Element {http://www.w3.org/2005/Atom}title at ead510>]
>>> title = tree.findall('{http://www.w3.org/2005/Atom}title')[0]
>>> title.text                                                   ④
'dive into '
>>> print(lxml.etree.tounicode(tree.getroot()))                  ⑤
<feed xmlns='http://www.w3.org/2005/Atom' xml:lang='en'>
  <title>dive into </title>
.
. [rest of serialization snipped for brevity]
.
~~~~

1.  To create a custom parser, instantiate the `lxml.etree.XMLParser`
    class. It can take [a number of different named
    arguments](http://codespeak.net/lxml/parsing.html#parser-options).
    The one we’re interested in here is the recover argument. When set
    to `True`, the XML parser will try its best to “recover” from
    wellformedness errors.
2.  To parse an XML document with your custom parser, pass the parser
    object as the second argument to the `parse()` function. Note that
    `lxml` does not raise an exception about the undefined `&hellip;`
    entity.
3.  The parser keeps a log of the wellformedness errors that it has
    encountered. (This is actually true regardless of whether it is set
    to recover from those errors or not.)
4.  Since it didn’t know what to do with the undefined `&hellip;`
    entity, the parser just silently dropped it. The text content of the
    `title` element becomes `'dive into '`.
5.  As you can see from the serialization, the `&hellip;` entity didn’t
    get moved; it was simply dropped.

It is important to reiterate that there is **no guarantee of
interoperability** with “recovering” XML parsers. A different parser
might decide that it recognized the `&hellip;` entity from HTML, and
replace it with `&amp;hellip;` instead. Is that “better”? Maybe. Is it
“more correct”? No, they are both equally incorrect. The correct
behavior (according to the XML specification) is to halt and catch fire.
If you’ve decided not to do that, you’re on your own.

⁂

Further Reading {#furtherreading}
---------------

-   [XML on Wikipedia.org](http://en.wikipedia.org/wiki/XML)
-   [The ElementTree XML
    API](http://docs.python.org/3.1/library/xml.etree.elementtree.html)
-   [Elements and Element Trees](http://effbot.org/zone/element.htm)
-   [XPath Support in
    ElementTree](http://effbot.org/zone/element-xpath.htm)
-   [The ElementTree iterparse
    Function](http://effbot.org/zone/element-iterparse.htm)
-   [`lxml`](http://codespeak.net/lxml/)
-   [Parsing XML and HTML with
    `lxml`](http://codespeak.net/lxml/1.3/parsing.html)
-   [XPath and XSLT with
    `lxml`](http://codespeak.net/lxml/1.3/xpathxslt.html)
-   [xmlwitch](http://github.com/galvez/xmlwitch/tree/master)

[☜](files.html "back to “Files”")
[☞](serializing.html "onward to “Serializing Python Objects”")

© 2001–11 [Mark Pilgrim](about.html)
