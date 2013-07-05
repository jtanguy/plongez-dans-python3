Packaging Python Libraries - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#packaging) ‣

Difficulty level: ♦♦♦♦♢

Packaging Python Libraries
==========================

> ❝ You’ll find the shame is like the pain; you only feel it once. ❞\
> — Marquise de Merteuil, [Dangerous
> Liaisons](http://www.imdb.com/title/tt0094947/quotes)

 

Diving In {#divingin}
---------

Real artists ship. Or so says Steve Jobs. Do you want to release a
Python script, library, framework, or application? Excellent. The world
needs more Python code. Python 3 comes with a packaging framework called
Distutils. Distutils is many things: a build tool (for you), an
installation tool (for your users), a package metadata format (for
search engines), and more. It integrates with the [Python Package
Index](http://pypi.python.org/) (“PyPI”), a central repository for open
source Python libraries.

All of these facets of Distutils center around the *setup script*,
traditionally called `setup.py`. In fact, you’ve already seen several
Distutils setup scripts in this book. You used Distutils to install
`httplib2` in [HTTP Web
Services](http-web-services.html#introducing-httplib2) and again to
install `chardet` in [Case Study: Porting `chardet` to Python
3](case-study-porting-chardet-to-python-3.html).

In this chapter, you’ll learn how the setup scripts for `chardet` and
`httplib2` work, and you’ll step through the process of releasing your
own Python software.

~~~~ {.pp}
# chardet's setup.py
from distutils.core import setup
setup(
    name = "chardet",
    packages = ["chardet"],
    version = "1.0.2",
    description = "Universal encoding detector",
    author = "Mark Pilgrim",
    author_email = "mark@diveintomark.org",
    url = "http://chardet.feedparser.org/",
    download_url = "http://chardet.feedparser.org/download/python3-chardet-1.0.1.tgz",
    keywords = ["encoding", "i18n", "xml"],
    classifiers = [
        "Programming Language :: Python",
        "Programming Language :: Python :: 3",
        "Development Status :: 4 - Beta",
        "Environment :: Other Environment",
        "Intended Audience :: Developers",
        "License :: OSI Approved :: GNU Library or Lesser General Public License (LGPL)",
        "Operating System :: OS Independent",
        "Topic :: Software Development :: Libraries :: Python Modules",
        "Topic :: Text Processing :: Linguistic",
        ],
    long_description = """\
Universal character encoding detector
-------------------------------------

Detects
 - ASCII, UTF-8, UTF-16 (2 variants), UTF-32 (4 variants)
 - Big5, GB2312, EUC-TW, HZ-GB-2312, ISO-2022-CN (Traditional and Simplified Chinese)
 - EUC-JP, SHIFT_JIS, ISO-2022-JP (Japanese)
 - EUC-KR, ISO-2022-KR (Korean)
 - KOI8-R, MacCyrillic, IBM855, IBM866, ISO-8859-5, windows-1251 (Cyrillic)
 - ISO-8859-2, windows-1250 (Hungarian)
 - ISO-8859-5, windows-1251 (Bulgarian)
 - windows-1252 (English)
 - ISO-8859-7, windows-1253 (Greek)
 - ISO-8859-8, windows-1255 (Visual and Logical Hebrew)
 - TIS-620 (Thai)

This version requires Python 3 or later; a Python 2 version is available separately.
"""
)
~~~~

> ☞`chardet` and `httplib2` are open source, but there’s no requirement
> that you release your own Python libraries under any particular
> license. The process described in this chapter will work for any
> Python software, regardless of license.

⁂

Things Distutils Can’t Do For You {#cantdo}
---------------------------------

Releasing your first Python package is a daunting process. (Releasing
your second one is a little easier.) Distutils tries to automate as much
of it as possible, but there are some things you simply must do
yourself.

-   **Choose a license**. This is a complicated topic, fraught with
    politics and peril. If you wish to release your software as open
    source, I humbly offer five pieces of advice:
    1.  Don’t write your own license.
    2.  Don’t write your own license.
    3.  Don’t write your own license.
    4.  It doesn’t need to be GPL, but [it needs to be
        GPL-compatible](http://www.dwheeler.com/essays/gpl-compatible.html).
    5.  Don’t write your own license.

-   **Classify your software** using the PyPI classification system.
    I’ll explain what this means later in this chapter.
-   **Write a “read me” file**. Don’t skimp on this. At a minimum, it
    should give your users an overview of what your software does and
    how to install it.

⁂

Directory Structure {#structure}
-------------------

To start packaging your Python software, you need to get your files and
directories in order. The `httplib2` directory looks like this:

~~~~ {.screen}
httplib2/                 ①
|
+--README.txt             ②
|
+--setup.py               ③
|
+--httplib2/              ④
   |
   +--__init__.py
   |
   +--iri2uri.py
~~~~

1.  Make a root directory to hold everything. Give it the same name as
    your Python module.
2.  To accomodate Windows users, your “read me” file should include a
    `.txt` extension, and it should use Windows-style carriage returns.
    Just because *you* use a fancy text editor that runs from the
    command line and includes its own macro language, that doesn’t mean
    you need to make life difficult for your users. (Your users use
    Notepad. Sad but true.) Even if you’re on Linux or Mac OS X, your
    fancy text editor undoubtedly has an option to save files with
    Windows-style carriage returns.
3.  Your Distutils setup script should be named `setup.py` unless you
    have a good reason not to. You do not have a good reason not to.
4.  If your Python software is a single `.py` file, you should put it in
    the root directory along with your “read me” file and your setup
    script. But `httplib2` is not a single `.py` file; it’s [a
    multi-file
    module](case-study-porting-chardet-to-python-3.html#multifile-modules).
    But that’s OK! Just put the `httplib2` directory in the root
    directory, so you have an `__init__.py` file within an `httplib2/`
    directory within the `httplib2/` root directory. That’s not a
    problem; in fact, it will simplify your packaging process.

The `chardet` directory looks slightly different. Like `httplib2`, it’s
[a multi-file
module](case-study-porting-chardet-to-python-3.html#multifile-modules),
so there’s a `chardet/` directory within the `chardet/` root directory.
In addition to the `README.txt` file, `chardet` has HTML-formatted
documentation in the `docs/` directory. The `docs/` directory contains
several `.html` and `.css` files and an `images/` subdirectory, which
contains several `.png` and `.gif` files. (This will be important
later.) Also, in keeping with the convention for (L)GPL-licensed
software, it has a separate file called `COPYING.txt` which contains the
complete text of the LGPL.

~~~~ {.nd}
chardet/
|
+--COPYING.txt
|
+--setup.py
|
+--README.txt
|
+--docs/
|  |
|  +--index.html
|  |
|  +--usage.html
|  |
|  +--images/ ...
|
+--chardet/
   |
   +--__init__.py
   |
   +--big5freq.py
   |
   +--...
~~~~

⁂

Writing Your Setup Script {#setuppy}
-------------------------

The Distutils setup script is a Python script. In theory, it can do
anything Python can do. In practice, it should do as little as possible,
in as standard a way as possible. Setup scripts should be boring. The
more exotic your installation process is, the more exotic your bug
reports will be.

The first line of every Distutils setup script is always the same:

~~~~ {.nd .pp}
from distutils.core import setup
~~~~

This imports the `setup()` function, which is the main entry point into
Distutils. 95% of all Distutils setup scripts consist of a single call
to `setup()` and nothing else. (I totally just made up that statistic,
but if your Distutils setup script is doing more than calling the
Distutils `setup()` function, you should have a good reason. Do you have
a good reason? I didn’t think so.)

The `setup()` function [can take dozens of
parameters](http://docs.python.org/3.1/distutils/apiref.html#distutils.core.setup).
For the sanity of everyone involved, you must use [named
arguments](your-first-python-program.html#optional-arguments) for every
parameter. This is not merely a convention; it’s a hard requirement.
Your setup script will crash if you try to call the `setup()` function
with non-named arguments.

The following named arguments are required:

-   **name**, the name of the package.
-   **version**, the version number of the package.
-   **author**, your full name.
-   **author\_email**, your email address.
-   **url**, the home page of your project. This can be your
    [PyPI](http://pypi.python.org/) package page if you don’t have a
    separate project website.

Although not required, I recommend that you also include the following
in your setup script:

-   **description**, a one-line summary of the project.
-   **long\_description**, a multi-line string in [reStructuredText
    format](http://docutils.sourceforge.net/rst.html).
    [PyPI](http://pypi.python.org/) converts this to HTML and displays
    it on your package page.
-   **classifiers**, a list of specially-formatted strings described in
    the next section.

> ☞Setup script metadata is defined in [PEP
> 314](http://www.python.org/dev/peps/pep-0314/).

Now let’s look at the `chardet` setup script. It has all of these
required and recommended parameters, plus one I haven’t mentioned yet:
`packages`.

~~~~ {.nd .pp}
from distutils.core import setup
setup(
    name = 'chardet',
    packages = ['chardet'],
    version = '1.0.2',
    description = 'Universal encoding detector',
    author='Mark Pilgrim',
    ...
)
~~~~

The `packages` parameter highlights an unfortunate vocabulary overlap in
the distribution process. We’ve been talking about the “package” as the
thing you’re building (and potentially listing in The Python “Package”
Index). But that’s not what this `packages` parameter refers to. It
refers to the fact that the `chardet` module is [a multi-file
module](case-study-porting-chardet-to-python-3.html#multifile-modules),
sometimes known as… a “package.” The `packages` parameter tells
Distutils to include the `chardet/` directory, its `__init__.py` file,
and all the other `.py` files that constitute the `chardet` module.
That’s kind of important; all this happy talk about documentation and
metadata is irrelevant if you forget to include the actual code!

⁂

Classifying Your Package {#trove}
------------------------

The Python Package Index (“PyPI”) contains thousands of Python
libraries. Proper classification metadata will allow people to find
yours more easily. PyPI lets you [browse packages by
classifier](http://pypi.python.org/pypi?:action=browse). You can even
select multiple classifiers to narrow your search. Classifiers are not
invisible metadata that you can just ignore!

To classify your software, pass a `classifiers` parameter to the
Distutils `setup()` function. The `classifiers` parameter is a list of
strings. These strings are *not* freeform. All classifier strings should
come from [this list on
PyPI](http://pypi.python.org/pypi?:action=list_classifiers).

Classifiers are optional. You can write a Distutils setup script without
any classifiers at all. **Don’t do that.** You should *always* include
at least these classifiers:

-   **Programming Language**. In particular, you should include both
    `"Programming Language :: Python"` and
    `"Programming Language :: Python :: 3"`. If you do not include
    these, your package will not show up in [this list of Python
    3-compatible
    libraries](http://pypi.python.org/pypi?:action=browse&c=533&show=all),
    which linked from the sidebar of every single page of
    `pypi.python.org`.
-   **License**. This is *the absolute first thing I look for* when I’m
    evaluating third-party libraries. Don’t make me hunt for this vital
    information. Don’t include more than one license classifier unless
    your software is explicitly available under multiple licenses. (And
    don’t release software under multiple licenses unless you’re forced
    to do so. And don’t force other people to do so. Licensing is enough
    of a headache; don’t make it worse.)
-   **Operating System**. If your software only runs on Windows (or Mac
    OS X, or Linux), I want to know sooner rather than later. If your
    software runs anywhere without any platform-specific code, use the
    classifier `"Operating System :: OS Independent"`. Multiple
    `Operating System` classifiers are only necessary if your software
    requires specific support for each platform. (This is not common.)

I also recommend that you include the following classifiers:

-   **Development Status**. Is your software beta quality? Alpha
    quality? Pre-alpha? Pick one. Be honest.
-   **Intended Audience**. Who would download your software? The most
    common choices are `Developers`, `End Users/Desktop`,
    `Science/Research`, and `System Administrators`.
-   **Framework**. If your software is a plugin for a larger Python
    framework like [Django](http://www.djangoproject.com/) or
    [Zope](http://www.zope.org/), include the appropriate `Framework`
    classifier. If not, omit it.
-   **Topic**. There are [a large number of topics to choose
    from](http://pypi.python.org/pypi?:action=list_classifiers); choose
    all that apply.

### Examples of Good Package Classifiers {#trove-examples}

By way of example, here are the classifiers for
[Django](http://pypi.python.org/pypi/Django/), a production-ready,
cross-platform, BSD-licensed web application framework that runs on your
web server. (Django is not yet compatible with Python 3, so the
`Programming Language :: Python :: 3` classifier is not listed.)

    Programming Language :: Python
    License :: OSI Approved :: BSD License
    Operating System :: OS Independent
    Development Status :: 5 - Production/Stable
    Environment :: Web Environment
    Framework :: Django
    Intended Audience :: Developers
    Topic :: Internet :: WWW/HTTP
    Topic :: Internet :: WWW/HTTP :: Dynamic Content
    Topic :: Internet :: WWW/HTTP :: WSGI
    Topic :: Software Development :: Libraries :: Python Modules

Here are the classifiers for
[`chardet`](http://pypi.python.org/pypi/chardet), the character encoding
detection library covered in [Case Study: Porting `chardet` to Python
3](case-study-porting-chardet-to-python-3.html). `chardet` is beta
quality, cross-platform, Python 3-compatible, LGPL-licensed, and
intended for developers to integrate into their own products.

    Programming Language :: Python
    Programming Language :: Python :: 3
    License :: OSI Approved :: GNU Library or Lesser General Public License (LGPL)
    Operating System :: OS Independent
    Development Status :: 4 - Beta
    Environment :: Other Environment
    Intended Audience :: Developers
    Topic :: Text Processing :: Linguistic
    Topic :: Software Development :: Libraries :: Python Modules

And here are the classifiers for
[`httplib2`](http://pypi.python.org/pypi/httplib2), the library featured
in the [HTTP Web Services](http-web-services.html) chapter. `httplib2`
is beta quality, cross-platform, MIT-licensed, and intended for Python
developers.

    Programming Language :: Python
    Programming Language :: Python :: 3
    License :: OSI Approved :: MIT License
    Operating System :: OS Independent
    Development Status :: 4 - Beta
    Environment :: Web Environment
    Intended Audience :: Developers
    Topic :: Internet :: WWW/HTTP
    Topic :: Software Development :: Libraries :: Python Modules

Specifying Additional Files With A Manifest {#manifest}
-------------------------------------------

By default, Distutils will include the following files in your release
package:

-   `README.txt`
-   `setup.py`
-   The `.py` files needed by the multi-file modules listed in the
    `packages` parameter
-   The individual `.py` files listed in the `py_modules` parameter

That will cover [all the files in the `httplib2` project](#structure).
But for the `chardet` project, we also want to include the `COPYING.txt`
license file and the entire `docs/` directory that contains images and
HTML files. To tell Distutils to include these additional files and
directories when it builds the `chardet` release package, you need a
*manifest file*.

A manifest file is a text file called `MANIFEST.in`. Place it in the
project’s root directory, next to `README.txt` and `setup.py`. Manifest
files are *not* Python scripts; they are text files that contain a
series of “commands” in a Distutils-defined format. Manifest commands
allow you to include or exclude specific files and directories.

This is the entire manifest file for the `chardet` project:

~~~~ {.nd}
include COPYING.txt                                ①
recursive-include docs *.html *.css *.png *.gif    ②
~~~~

1.  The first line is self-explanatory: include the `COPYING.txt` file
    from the project’s root directory.
2.  The second line is a bit more complicated. The `recursive-include`
    command takes a directory name and one or more filenames. The
    filenames aren’t limited to specific files; they can include
    wildcards. This line means “See that `docs/` directory in the
    project’s root directory? Look in there (recursively) for `.html`,
    `.css`, `.png`, and `.gif` files. I want all of them in my release
    package.”

All manifest commands preserve the directory structure that you set up
in your project directory. That `recursive-include` command is not going
to put a bunch of `.html` and `.png` files in the root directory of the
release package. It’s going to maintain the existing `docs/` directory
structure, but only include those files inside that directory that match
the given wildcards. (I didn’t mention it earlier, but the `chardet`
documentation is actually written in XML and converted to HTML by a
separate script. I don’t want to include the XML files in the release
package, just the HTML and the images.)

> ☞Manifest files have their own unique format. See [Specifying the
> files to
> distribute](http://docs.python.org/3.1/distutils/sourcedist.html#manifest)
> and [the manifest template
> commands](http://docs.python.org/3.1/distutils/commandref.html#sdist-cmd)
> for details.

To reiterate: you only need to create a manifest file if you want to
include files that Distutils doesn’t include by default. If you do need
a manifest file, it should only include the files and directories that
Distutils wouldn’t otherwise find on its own.

Checking Your Setup Script for Errors {#check}
-------------------------------------

There’s a lot to keep track of. Distutils comes with a built-in
validation command that checks that all the required metadata is present
in your setup script. For example, if you forget to include the
`version` parameter, Distutils will remind you.

~~~~ {.screen}
c:\Users\pilgrim\chardet> c:\python31\python.exe setup.py check
running check
warning: check: missing required meta-data: version
~~~~

Once you include a `version` parameter (and all the other required bits
of metadata), the `check` command will look like this:

~~~~ {.screen}
c:\Users\pilgrim\chardet> c:\python31\python.exe setup.py check
running check
~~~~

⁂

Creating a Source Distribution {#sdist}
------------------------------

Distutils supports building multiple types of release packages. At a
minimum, you should build a “source distribution” that contains your
source code, your Distutils setup script, your “read me” file, and
whatever [additional files you want to include](#manifest). To build a
source distribution, pass the `sdist` command to your Distutils setup
script.

~~~~ {.screen}
c:\Users\pilgrim\chardet> c:\python31\python.exe setup.py sdist
running sdist
running check
reading manifest template 'MANIFEST.in'
writing manifest file 'MANIFEST'
creating chardet-1.0.2
creating chardet-1.0.2\chardet
creating chardet-1.0.2\docs
creating chardet-1.0.2\docs\images
copying files to chardet-1.0.2...
copying COPYING -> chardet-1.0.2
copying README.txt -> chardet-1.0.2
copying setup.py -> chardet-1.0.2
copying chardet\__init__.py -> chardet-1.0.2\chardet
copying chardet\big5freq.py -> chardet-1.0.2\chardet
...
copying chardet\universaldetector.py -> chardet-1.0.2\chardet
copying chardet\utf8prober.py -> chardet-1.0.2\chardet
copying docs\faq.html -> chardet-1.0.2\docs
copying docs\history.html -> chardet-1.0.2\docs
copying docs\how-it-works.html -> chardet-1.0.2\docs
copying docs\index.html -> chardet-1.0.2\docs
copying docs\license.html -> chardet-1.0.2\docs
copying docs\supported-encodings.html -> chardet-1.0.2\docs
copying docs\usage.html -> chardet-1.0.2\docs
copying docs\images\caution.png -> chardet-1.0.2\docs\images
copying docs\images\important.png -> chardet-1.0.2\docs\images
copying docs\images\note.png -> chardet-1.0.2\docs\images
copying docs\images\permalink.gif -> chardet-1.0.2\docs\images
copying docs\images\tip.png -> chardet-1.0.2\docs\images
copying docs\images\warning.png -> chardet-1.0.2\docs\images
creating dist
creating 'dist\chardet-1.0.2.zip' and adding 'chardet-1.0.2' to it
adding 'chardet-1.0.2\COPYING'
adding 'chardet-1.0.2\PKG-INFO'
adding 'chardet-1.0.2\README.txt'
adding 'chardet-1.0.2\setup.py'
adding 'chardet-1.0.2\chardet\big5freq.py'
adding 'chardet-1.0.2\chardet\big5prober.py'
...
adding 'chardet-1.0.2\chardet\universaldetector.py'
adding 'chardet-1.0.2\chardet\utf8prober.py'
adding 'chardet-1.0.2\chardet\__init__.py'
adding 'chardet-1.0.2\docs\faq.html'
adding 'chardet-1.0.2\docs\history.html'
adding 'chardet-1.0.2\docs\how-it-works.html'
adding 'chardet-1.0.2\docs\index.html'
adding 'chardet-1.0.2\docs\license.html'
adding 'chardet-1.0.2\docs\supported-encodings.html'
adding 'chardet-1.0.2\docs\usage.html'
adding 'chardet-1.0.2\docs\images\caution.png'
adding 'chardet-1.0.2\docs\images\important.png'
adding 'chardet-1.0.2\docs\images\note.png'
adding 'chardet-1.0.2\docs\images\permalink.gif'
adding 'chardet-1.0.2\docs\images\tip.png'
adding 'chardet-1.0.2\docs\images\warning.png'
removing 'chardet-1.0.2' (and everything under it)
~~~~

Several things to note here:

-   Distutils noticed the manifest file (`MANIFEST.in`).
-   Distutils successfully parsed the manifest file and added the
    additional files we wanted — `COPYING.txt` and the HTML and image
    files in the `docs/` directory.
-   If you look in your project directory, you’ll see that Distutils
    created a `dist/` directory. Within the `dist/` directory the `.zip`
    file that you can distribute.

~~~~ {.screen}
c:\Users\pilgrim\chardet> dir dist
 Volume in drive C has no label.
 Volume Serial Number is DED5-B4F8

 Directory of c:\Users\pilgrim\chardet\dist

07/30/2009  06:29 PM    <DIR>          .
07/30/2009  06:29 PM    <DIR>          ..
07/30/2009  06:29 PM           206,440 chardet-1.0.2.zip
               1 File(s)        206,440 bytes
               2 Dir(s)  61,424,635,904 bytes free
~~~~

⁂

Creating a Graphical Installer {#bdist}
------------------------------

In my opinion, every Python library deserves a graphical installer for
Windows users. It’s easy to make (even if you don’t run Windows
yourself), and Windows users appreciate it.

Distutils can [create a graphical Windows installer for
you](http://docs.python.org/3.1/distutils/builtdist.html#creating-windows-installers),
by passing the `bdist_wininst` command to your Distutils setup script.

~~~~ {.screen}
c:\Users\pilgrim\chardet> c:\python31\python.exe setup.py bdist_wininst
running bdist_wininst
running build
running build_py
creating build
creating build\lib
creating build\lib\chardet
copying chardet\big5freq.py -> build\lib\chardet
copying chardet\big5prober.py -> build\lib\chardet
...
copying chardet\universaldetector.py -> build\lib\chardet
copying chardet\utf8prober.py -> build\lib\chardet
copying chardet\__init__.py -> build\lib\chardet
installing to build\bdist.win32\wininst
running install_lib
creating build\bdist.win32
creating build\bdist.win32\wininst
creating build\bdist.win32\wininst\PURELIB
creating build\bdist.win32\wininst\PURELIB\chardet
copying build\lib\chardet\big5freq.py -> build\bdist.win32\wininst\PURELIB\chardet
copying build\lib\chardet\big5prober.py -> build\bdist.win32\wininst\PURELIB\chardet
...
copying build\lib\chardet\universaldetector.py -> build\bdist.win32\wininst\PURELIB\chardet
copying build\lib\chardet\utf8prober.py -> build\bdist.win32\wininst\PURELIB\chardet
copying build\lib\chardet\__init__.py -> build\bdist.win32\wininst\PURELIB\chardet
running install_egg_info
Writing build\bdist.win32\wininst\PURELIB\chardet-1.0.2-py3.1.egg-info
creating 'c:\users\pilgrim\appdata\local\temp\tmp2f4h7e.zip' and adding '.' to it
adding 'PURELIB\chardet-1.0.2-py3.1.egg-info'
adding 'PURELIB\chardet\big5freq.py'
adding 'PURELIB\chardet\big5prober.py'
...
adding 'PURELIB\chardet\universaldetector.py'
adding 'PURELIB\chardet\utf8prober.py'
adding 'PURELIB\chardet\__init__.py'
removing 'build\bdist.win32\wininst' (and everything under it)
c:\Users\pilgrim\chardet> dir dist
c:\Users\pilgrim\chardet>dir dist
 Volume in drive C has no label.
 Volume Serial Number is AADE-E29F

 Directory of c:\Users\pilgrim\chardet\dist

07/30/2009  10:14 PM    <DIR>          .
07/30/2009  10:14 PM    <DIR>          ..
07/30/2009  10:14 PM           371,236 chardet-1.0.2.win32.exe
07/30/2009  06:29 PM           206,440 chardet-1.0.2.zip
               2 File(s)        577,676 bytes
               2 Dir(s)  61,424,070,656 bytes free
~~~~

### Building Installable Packages for Other Operating Systems {#linux}

Distutils can help you [build installable packages for Linux
users](http://docs.python.org/3.1/distutils/builtdist.html#creating-rpm-packages).
In my opinion, this probably isn’t worth your time. If you want your
software distributed for Linux, your time would be better spent working
with community members who specialize in packaging software for major
Linux distributions.

For example, my `chardet` library is [in the Debian GNU/Linux
repositories](http://packages.debian.org/python-chardet) (and therefore
[in the Ubuntu repositories](http://packages.ubuntu.com/python-chardet)
as well). I had nothing to do with this; the packages just showed up
there one day. The Debian community has [their own policies for
packaging Python
libraries](http://www.debian.org/doc/packaging-manuals/python-policy/),
and the Debian `python-chardet` package is designed to follow these
conventions. And since the package lives in Debian’s repositories,
Debian users will receive security updates and/or new versions,
depending on the system-wide settings they’ve chosen to manage their own
computers.

The Linux packages that Distutils builds offer none of these advantages.
Your time is better spent elsewhere.

⁂

Adding Your Software to The Python Package Index {#pypi}
------------------------------------------------

Uploading software to the Python Package Index is a three step process.

1.  Register yourself
2.  Register your software
3.  Upload the packages you created with `setup.py sdist` and
    `setup.py bdist_*`

To register yourself, go to [the PyPI user registration
page](http://pypi.python.org/pypi?:action=register_form). Enter your
desired username and password, provide a valid email address, and click
the `Register` button. (If you have a PGP or GPG key, you can also
provide that. If you don’t have one or don’t know what that means, don’t
worry about it.) Check your email; within a few minutes, you should
receive a message from PyPI with a validation link. Click the link to
complete the registration process.

Now you need to register your software with PyPI and upload it. You can
do this all in one step.

~~~~ {.screen}
c:\Users\pilgrim\chardet> c:\python31\python.exe setup.py register sdist bdist_wininst upload  ①
running register
We need to know who you are, so please choose either:
 1. use your existing login,
 2. register as a new user,
 3. have the server generate a new password for you (and email it to you), or
 4. quit
Your selection [default 1]:  1                                                                 ②
Username: MarkPilgrim                                                                          ③
Password:
Registering chardet to http://pypi.python.org/pypi                                             ④
Server response (200): OK
running sdist                                                                                  ⑤
... output trimmed for brevity ...
running bdist_wininst                                                                          ⑥
... output trimmed for brevity ...
running upload                                                                                 ⑦
Submitting dist\chardet-1.0.2.zip to http://pypi.python.org/pypi
Server response (200): OK
Submitting dist\chardet-1.0.2.win32.exe to http://pypi.python.org/pypi
Server response (200): OK
I can store your PyPI login so future submissions will be faster.
(the login will be stored in c:\home\.pypirc)
Save your login (y/N)?n                                                                        ⑧
~~~~

1.  When you release your project for the first time, Distutils will add
    your software to the Python Package Index and give it its own URL.
    Every time after that, it will simply update the project metadata
    with any changes you may have made in your `setup.py` parameters.
    Next, it builds a source distribution (`sdist`) and a Windows
    installer (`bdist_wininst`), then uploads them to PyPI (`upload`).
2.  Type 1 or just press ENTER to select “use your existing login.”
3.  Enter the username and password you selected on the [the PyPI user
    registration
    page](http://pypi.python.org/pypi?:action=register_form). Distuils
    will not echo your password; it will not even echo asterisks in
    place of characters. Just type your password and press ENTER.
4.  Distutils registers your package with the Python Package Index…
5.  …builds your source distribution…
6.  …builds your Windows installer…
7.  …and uploads them both to the Python Package Index.
8.  If you want to automate the process of releasing new versions, you
    need to save your PyPI credentials in a local file. This is
    completely insecure and completely optional.

Congratulations, you now have your own page on the Python Package Index!
The address is `http://pypi.python.org/pypi/NAME`, where *NAME* is the
string you passed in the name parameter in your `setup.py` file.

If you want to release a new version, just update your `setup.py` with
the new version number, then run the same upload command again:

~~~~ {.nd .screen}
c:\Users\pilgrim\chardet> c:\python31\python.exe setup.py register sdist bdist_wininst upload
~~~~

⁂

The Many Possible Futures of Python Packaging {#future}
---------------------------------------------

Distutils is not the be-all and end-all of Python packaging, but as of
this writing (August 2009), it’s the only packaging framework that works
in Python 3. There are a number of other frameworks for Python 2; some
focus on installation, others on testing and deployment. Some or all of
these may end up being ported to Python 3 in the future.

These frameworks focus on installation:

-   [Setuptools](http://pypi.python.org/pypi/setuptools)
-   [Pip](http://pypi.python.org/pypi/pip)
-   [Distribute](http://bitbucket.org/tarek/distribute/)

These focus on testing and deployment:

-   [`virtualenv`](http://pypi.python.org/pypi/virtualenv)
-   [`zc.buildout`](http://pypi.python.org/pypi/zc.buildout)
-   [Paver](http://www.blueskyonmars.com/projects/paver/)
-   [Fabric](http://fabfile.org/)
-   [`py2exe`](http://www.py2exe.org/)

⁂

Further Reading {#furtherreading}
---------------

On Distutils:

-   [Distributing Python Modules with
    Distutils](http://docs.python.org/3.1/distutils/)
-   [Core Distutils
    functionality](http://docs.python.org/3.1/distutils/apiref.html#module-distutils.core)
    lists all the possible arguments to the `setup()` function
-   [Distutils Cookbook](http://wiki.python.org/moin/Distutils/Cookbook)
-   [PEP 370: Per user `site-packages`
    directory](http://www.python.org/dev/peps/pep-0370/)
-   [PEP 370 and “environment
    stew”](http://jessenoller.com/2009/07/19/pep-370-per-user-site-packages-and-environment-stew/)

On other packaging frameworks:

-   [The Python packaging
    ecosystem](http://groups.google.com/group/django-developers/msg/5407cdb400157259)
-   [On packaging](http://www.b-list.org/weblog/2008/dec/14/packaging/)
-   [A few corrections to “On
    packaging”](http://blog.ianbicking.org/2008/12/14/a-few-corrections-to-on-packaging/)
-   [Why I like Pip](http://www.b-list.org/weblog/2008/dec/15/pip/)
-   [Python packaging: a few
    observations](http://cournape.wordpress.com/2009/04/01/python-packaging-a-few-observations-cabal-for-a-solution/)
-   [Nobody expects Python
    packaging!](http://jacobian.org/writing/nobody-expects-python-packaging/)

[☜](case-study-porting-chardet-to-python-3.html "back to “Case Study: Porting chardet to Python 3”")
[☞](porting-code-to-python-3-with-2to3.html "onward to “Porting Code to Python 3 with 2to3”")

© 2001–11 [Mark Pilgrim](about.html)
