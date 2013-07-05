Installing Python - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#installing-python) ‣

Difficulty level: ♦♢♢♢♢

Installing Python
=================

> ❝ *Tempora mutantur nos et mutamur in illis.* (Times change, and we
> change with them.) ❞\
> — ancient Roman proverb

 

Diving In {#divingin}
---------

Before you can start programming in Python 3, you need to install it. Or
do you?

Which Python Is Right For You? {#which}
------------------------------

If you're using an account on a hosted server, your ISP may have already
installed Python 3. If you’re running Linux at home, you may already
have Python 3, too. Most popular GNU/Linux distributions come with
Python 2 in the default installation; a small but growing number of
distributions also include Python 3. Mac OS X includes a command-line
version of Python 2, but as of this writing it does not include Python
3. Microsoft Windows does not come with any version of Python. But don’t
despair! You can point-and-click your way through installing Python,
regardless of what operating system you have.

The easiest way to check for Python 3 on your Linux or Mac OS X system
is [from the command
line](troubleshooting.html#getting-to-the-command-line). Once you’re at
a command line prompt, just type python3 (all lowercase, no spaces),
press ENTER, and see what happens. On my home Linux system, Python 3.1
is already installed, and this command gets me into the *Python
interactive shell*.

~~~~ {.nd .screen .cmdline}
mark@atlantis:~$ python3
Python 3.1 (r31:73572, Jul 28 2009, 06:52:23) 
[GCC 4.2.4 (Ubuntu 4.2.4-1ubuntu4)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
~~~~

(Type exit() and press ENTER to exit the Python interactive shell.)

My [web hosting provider](http://cornerhost.com/) also runs Linux and
provides command-line access, but my server does not have Python 3
installed. (Boo!)

~~~~ {.nd .screen .cmdline}
mark@manganese:~$ python3
bash: python3: command not found
~~~~

So back to the question that started this section, “Which Python is
right for you?” Whichever one runs on the computer you already have.

[Read on for Windows instructions, or skip to [Installing on Mac OS
X](#macosx), [Installing on Ubuntu Linux](#ubuntu), or [Installing on
Other Platforms](#other).]

⁂

Installing on Microsoft Windows {#windows}
-------------------------------

Windows comes in two architectures these days: 32-bit and 64-bit. Of
course, there are lots of different *versions* of Windows — XP, Vista,
Windows 7 — but Python runs on all of them. The more important
distinction is 32-bit v. 64-bit. If you have no idea what architecture
you’re running, it’s probably 32-bit.

Visit [`python.org/download/`](http://python.org/download/) and download
the appropriate Python 3 Windows installer for your architecture. Your
choices will look something like this:

-   **Python 3.1 Windows installer** (Windows binary — does not include
    source)
-   **Python 3.1 Windows AMD64 installer** (Windows AMD64 binary — does
    not include source)

I don’t want to include direct download links here, because minor
updates of Python happen all the time and I don’t want to be responsible
for you missing important updates. You should always install the most
recent version of Python 3.x unless you have some esoteric reason not
to.

1.  ![[Windows dialog: open file security
    warning]](i/win-install-0-security-warning.png)

    Once your download is complete, double-click the `.msi` file.
    Windows will pop up a security alert, since you’re about to be
    running executable code. The official Python installer is digitally
    signed by the [Python Software
    Foundation](http://www.python.org/psf/), the non-profit corporation
    that oversees Python development. Don’t accept imitations!

    Click the `Run` button to launch the Python 3 installer.

2.  ![[Python installer: select whether to install Python 3.1 for all
    users of this computer]](i/win-install-1-all-users-or-just-me.png)

    The first question the installer will ask you is whether you want to
    install Python 3 for all users or just for you. The default choice
    is “install for all users,” which is the best choice unless you have
    a good reason to choose otherwise. (One possible reason why you
    would want to “install just for me” is that you are installing
    Python on your company’s computer and you don’t have administrative
    rights on your Windows account. But then, why are you installing
    Python without permission from your company’s Windows administrator?
    Don’t get me in trouble here!)

    Click the `Next` button to accept your choice of installation type.

3.  ![[Python installer: select destination
    directory]](i/win-install-2-destination-directory.png)

    Next, the installer will prompt you to choose a destination
    directory. The default for all versions of Python 3.1.x is
    `C:\Python31\`, which should work well for most users unless you
    have a specific reason to change it. If you maintain a separate
    drive letter for installing applications, you can browse to it using
    the embedded controls, or simply type the pathname in the box below.
    You are not limited to installing Python on the `C:` drive; you can
    install it on any drive, in any folder.

    Click the `Next` button to accept your choice of destination
    directory.

4.  ![[Python installer: customize Python
    3.1]](i/win-install-3-customize.png)

    The next page looks complicated, but it’s not really. Like many
    installers, you have the option not to install every single
    component of Python 3. If disk space is especially tight, you can
    exclude certain components.

    -   **Register Extensions** allows you to double-click Python
        scripts (`.py` files) and run them. Recommended but not
        required. (This option doesn’t require any disk space, so there
        is little point in excluding it.)
    -   **Tcl/Tk** is the graphics library used by the Python Shell,
        which you will use throughout this book. I strongly recommend
        keeping this option.
    -   **Documentation** installs a help file that contains much of the
        information on [`docs.python.org`](http://docs.python.org/).
        Recommended if you are on dialup or have limited Internet
        access.
    -   **Utility Scripts** includes the `2to3.py` script which you’ll
        learn about [later in this
        book](case-study-porting-chardet-to-python-3.html). Required if
        you want to learn about migrating existing Python 2 code to
        Python 3. If you have no existing Python 2 code, you can skip
        this option.
    -   **Test Suite** is a collection of scripts used to test the
        Python interpreter itself. We will not use it in this book, nor
        have I ever used it in the course of programming in Python.
        Completely optional.

5.  ![[Python installer: disk space
    requirements]](i/win-install-3a-disk-usage.png)

    If you’re unsure how much disk space you have, click the
    `Disk Usage` button. The installer will list your drive letters,
    compute how much space is available on each drive, and calculate how
    much would be left after installation.

    Click the `OK` button to return to the “Customizing Python” page.

6.  ![[Python installer: removing Test Suite option will save 7908KB on
    your hard drive]](i/win-install-3b-test-suite.png)

    If you decide to exclude an option, select the drop-down button
    before the option and select “Entire feature will be unavailable.”
    For example, excluding the test suite will save you a whopping
    7908KB of disk space.

    Click the `Next` button to accept your choice of options.

7.  ![[Python installer: progress meter]](i/win-install-4-copying.png)

    The installer will copy all the necessary files to your chosen
    destination directory. (This happens so quickly, I had to try it
    three times to even get a screenshot of it!)

8.  ![[Python installer: installation completed. Special Windows thanks
    to Mark Hammond, without whose years of freely shared Windows
    expertise, Python for Windows would still be Python for
    DOS.]](i/win-install-5-finish.png)

    Click the `Finish` button to exit the installer.

9.  ![[Windows Python Shell, a graphical interactive shell for
    Python]](i/win-interactive-shell.png)

    In your `Start` menu, there should be a new item called
    `Python 3.1`. Within that, there is a program called IDLE. Select
    this item to run the interactive Python Shell.

[Skip to [using the Python Shell](#idle)]

⁂

Installing on Mac OS X {#macosx}
----------------------

All modern Macintosh computers use the Intel chip (like most Windows
PCs). Older Macs used PowerPC chips. You don’t need to understand the
difference, because there’s just one Mac Python installer for all Macs.

Visit [`python.org/download/`](http://python.org/download/) and download
the Mac installer. It will be called something like **Python 3.1 Mac
Installer Disk Image**, although the version number may vary. Be sure to
download version 3.x, not 2.x.

1.  ![[contents of Python installer disk
    image]](i/mac-install-0-dmg-contents.png)

    Your browser should automatically mount the disk image and open a
    Finder window to show you the contents. (If this doesn’t happen,
    you’ll need to find the disk image in your downloads folder and
    double-click to mount it. It will be named something like
    `python-3.1.dmg`.) The disk image contains a number of text files
    (`Build.txt`, `License.txt`, `ReadMe.txt`), and the actual installer
    package, `Python.mpkg`.

    Double-click the `Python.mpkg` installer package to launch the Mac
    Python installer.

2.  ![[Python installer: welcome screen]](i/mac-install-1-welcome.png)

    The first page of the installer gives a brief description of Python
    itself, then refers you to the `ReadMe.txt` file (which you didn’t
    read, did you?) for more details.

    Click the `Continue` button to move along.

3.  ![[Python installer: information about supported architectures, disk
    space, and acceptable destination
    folders]](i/mac-install-2-information.png)

    The next page actually contains some important information: Python
    requires Mac OS X 10.3 or later. If you are still running Mac OS X
    10.2, you should really upgrade. Apple no longer provides security
    updates for your operating system, and your computer is probably at
    risk if you ever go online. Also, you can’t run Python 3.

    Click the `Continue` button to advance.

4.  ![[Python installer: software license
    agreement]](i/mac-install-3-license.png)

    Like all good installers, the Python installer displays the software
    license agreement. Python is open source, and its license is
    [approved by the Open Source
    Initiative](http://opensource.org/licenses/). Python has had a
    number of owners and sponsors throughout its history, each of which
    has left its mark on the software license. But the end result is
    this: Python is open source, and you may use it on any platform, for
    any purpose, without fee or obligation of reciprocity.

    Click the `Continue` button once again.

5.  ![[Python installer: dialog to accept license
    agreement]](i/mac-install-4-license-dialog.png)

    Due to quirks in the standard Apple installer framework, you must
    “agree” to the software license in order to complete the
    installation. Since Python is open source, you are really “agreeing”
    that the license is granting you additional rights, rather than
    taking them away.

    Click the `Agree` button to continue.

6.  ![[Python installer: standard install
    screen]](i/mac-install-5-standard-install.png)

    The next screen allows you to change your install location. You
    **must** install Python on your boot drive, but due to limitations
    of the installer, it does not enforce this. In truth, I have never
    had the need to change the install location.

    From this screen, you can also customize the installation to exclude
    certain features. If you want to do this, click the `Customize`
    button; otherwise click the `Install` button.

7.  ![[Python installer: custom install
    screen]](i/mac-install-6-custom-install.png)

    If you choose a Custom Install, the installer will present you with
    the following list of features:

    -   **Python Framework**. This is the guts of Python, and is both
        selected and disabled because it must be installed.
    -   **GUI Applications** includes IDLE, the graphical Python Shell
        which you will use throughout this book. I strongly recommend
        keeping this option selected.
    -   **UNIX command-line tools** includes the command-line `python3`
        application. I strongly recommend keeping this option, too.
    -   **Python Documentation** contains much of the information on
        [`docs.python.org`](http://docs.python.org/). Recommended if you
        are on dialup or have limited Internet access.
    -   **Shell profile updater** controls whether to update your shell
        profile (used in `Terminal.app`) to ensure that this version of
        Python is on the search path of your shell. You probably don’t
        need to change this.
    -   **Fix system Python** should not be changed. (It tells your Mac
        to use Python 3 as the default Python for all scripts, including
        built-in system scripts from Apple. This would be very bad,
        since most of those scripts are written for Python 2, and they
        would fail to run properly under Python 3.)

    Click the `Install` button to continue.

8.  ![[Python installer: dialog to enter administrative
    password]](i/mac-install-7-admin-password.png)

    Because it installs system-wide frameworks and binaries in
    `/usr/local/bin/`, the installer will ask you for an administrative
    password. There is no way to install Mac Python without
    administrator privileges.

    Click the `OK` button to begin the installation.

9.  ![[Python installer: progress meter]](i/mac-install-8-progress.png)

    The installer will display a progress meter while it installs the
    features you’ve selected.

10. ![[Python installer: install
    succeeded]](i/mac-install-9-succeeded.png)

    Assuming all went well, the installer will give you a big green
    checkmark to tell you that the installation completed successfully.

    Click the `Close` button to exit the installer.

11. ![[contents of /Applications/Python 3.1/
    folder]](i/mac-install-10-application-folder.png)

    Assuming you didn’t change the install location, you can find the
    newly installed files in the `Python 3.1` folder within your
    `/Applications` folder. The most important piece is IDLE, the
    graphical Python Shell.

    Double-click IDLE to launch the Python Shell.

12. ![[Mac Python Shell, a graphical interactive shell for
    Python]](i/mac-interactive-shell.png)

    The Python Shell is where you will spend most of your time exploring
    Python. Examples throughout this book will assume that you can find
    your way into the Python Shell.

[Skip to [using the Python Shell](#idle)]

⁂

Installing on Ubuntu Linux {#ubuntu}
--------------------------

Modern Linux distributions are backed by vast repositories of
precompiled applications, ready to install. The exact details vary by
distribution. In Ubuntu Linux, the easiest way to install Python 3 is
through the `Add/Remove` application in your `Applications` menu.

1.  ![[Add/Remove: Canonical-maintained
    applications]](i/ubu-install-0-add-remove-programs.png)

    When you first launch the `Add/Remove` application, it will show you
    a list of preselected applications in different categories. Some are
    already installed; most are not. Because the repository contains
    over 10,000 applications, there are different filters you can apply
    to see small parts of the repository. The default filter is
    “Canonical-maintained applications,” which is a small subset of the
    total number of applications that are officially supported by
    Canonical, the company that creates and maintains Ubuntu Linux.

2.  ![[Add/Remove: all open source
    applications]](i/ubu-install-1-all-open-source-applications.png)

    Python 3 is not maintained by Canonical, so the first step is to
    drop down this filter menu and select “All Open Source
    applications.”

3.  ![[Add/Remove: search for Python
    3]](i/ubu-install-2-search-python-3.png)

    Once you’ve widened the filter to include all open source
    applications, use the Search box immediately after the filter menu
    to search for Python 3.

4.  ![[Add/Remove: select Python 3.0
    package]](i/ubu-install-3-select-python-3.png)

    Now the list of applications narrows to just those matching Python
    3. You’re going to check two packages. The first is `Python (v3.0)`.
    This contains the Python interpreter itself.

5.  ![[Add/Remove: select IDLE for Python 3.0
    package]](i/ubu-install-4-select-idle.png)

    The second package you want is immediately above:
    `IDLE (using Python-3.0)`. This is a graphical Python Shell that you
    will use throughout this book.

    After you’ve checked those two packages, click the `Apply Changes`
    button to continue.

6.  ![[Add/Remove: apply changes]](i/ubu-install-5-apply-changes.png)

    The package manager will ask you to confirm that you want to add
    both `IDLE (using Python-3.0)` and `Python (v3.0)`.

    Click the `Apply` button to continue.

7.  ![[Add/Remove: download progress
    meter]](i/ubu-install-6-download-progress.png)

    The package manager will show you a progress meter while it
    downloads the necessary packages from Canonical’s Internet
    repository.

8.  ![[Add/Remove: installation progress
    meter]](i/ubu-install-7-install-progress.png)

    Once the packages are downloaded, the package manager will
    automatically begin installing them.

9.  ![[Add/Remove: new applications have been
    installed]](i/ubu-install-8-success.png)

    If all went well, the package manager will confirm that both
    packages were successfully installed. From here, you can
    double-click IDLE to launch the Python Shell, or click the `Close`
    button to exit the package manager.

    You can always relaunch the Python Shell by going to your
    `Applications` menu, then the `Programming` submenu, and selecting
    IDLE.

10. ![[Linux Python Shell, a graphical interactive shell for
    Python]](i/ubu-interactive-shell.png)

    The Python Shell is where you will spend most of your time exploring
    Python. Examples throughout this book will assume that you can find
    your way into the Python Shell.

[Skip to [using the Python Shell](#idle)]

⁂

Installing on Other Platforms {#other}
-----------------------------

Python 3 is available on a number of different platforms. In particular,
it is available in virtually every Linux, BSD, and Solaris-based
distribution. For example, RedHat Linux uses the `yum` package manager.
FreeBSD has its [ports and packages
collection](http://www.freebsd.org/ports/), SUSE has `zypper`, and
Solaris has `pkgadd`. A quick web search for `Python 3` + *your
operating system* should tell you whether a Python 3 package is
available, and if so, how to install it.

⁂

Using The Python Shell {#idle}
----------------------

The Python Shell is where you can explore Python syntax, get interactive
help on commands, and debug short programs. The graphical Python Shell
(named IDLE) also contains a decent text editor that supports Python
syntax coloring and integrates with the Python Shell. If you don’t
already have a favorite text editor, you should give IDLE a try.

First things first. The Python Shell itself is an amazing interactive
playground. Throughout this book, you’ll see examples like this:

~~~~ {.nd .screen}
>>> 1 + 1
2
~~~~

The three angle brackets, \>\>\>, denote the Python Shell prompt. Don’t
type that part. That’s just to let you know that this example is meant
to be followed in the Python Shell.

1 + 1 is the part you type. You can type any valid Python expression or
command in the Python Shell. Don’t be shy; it won’t bite! The worst that
will happen is you’ll get an error message. Commands get executed
immediately (once you press ENTER); expressions get evaluated
immediately, and the Python Shell prints out the result.

2 is the result of evaluating this expression. As it happens, 1 + 1 is a
valid Python expression. The result, of course, is 2.

Let’s try another one.

~~~~ {.nd .screen}
>>> print('Hello world!')
Hello world!
~~~~

Pretty simple, no? But there’s lots more you can do in the Python shell.
If you ever get stuck — you can’t remember a command, or you can’t
remember the proper arguments to pass a certain function — you can get
interactive help in the Python Shell. Just type help and press ENTER.

~~~~ {.nd .screen}
>>> help
Type help() for interactive help, or help(object) for help about object.
~~~~

There are two modes of help. You can get help about a single object,
which just prints out the documentation and returns you to the Python
Shell prompt. You can also enter *help mode*, where instead of
evaluating Python expressions, you just type keywords or command names
and it will print out whatever it knows about that command.

To enter the interactive help mode, type help() and press ENTER.

~~~~ {.nd .screen}
>>> help()
Welcome to Python 3.0!  This is the online help utility.

If this is your first time using Python, you should definitely check out
the tutorial on the Internet at http://docs.python.org/tutorial/.

Enter the name of any module, keyword, or topic to get help on writing
Python programs and using Python modules.  To quit this help utility and
return to the interpreter, just type "quit".

To get a list of available modules, keywords, or topics, type "modules",
"keywords", or "topics".  Each module also comes with a one-line summary
of what it does; to list the modules whose summaries contain a given word
such as "spam", type "modules spam".

help> 
~~~~

Note how the prompt changes from \>\>\> to help\>. This reminds you that
you’re in the interactive help mode. Now you can enter any keyword,
command, module name, function name — pretty much anything Python
understands — and read documentation on it.

~~~~ {.screen}
help> print                                                                 ①
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file: a file-like object (stream); defaults to the current sys.stdout.
    sep:  string inserted between values, default a space.
    end:  string appended after the last value, default a newline.

help> PapayaWhip                                                            ②
no Python documentation found for 'PapayaWhip'

help> quit                                                                  ③

You are now leaving help and returning to the Python interpreter.
If you want to ask for help on a particular object directly from the
interpreter, you can type "help(object)".  Executing "help('string')"
has the same effect as typing a particular string at the help> prompt.
>>>                                                                         ④
~~~~

1.  To get documentation on the `print()` function, just type print and
    press ENTER. The interactive help mode will display something akin
    to a man page: the function name, a brief synopsis, the function’s
    arguments and their default values, and so on. If the documentation
    seems opaque to you, don’t panic. You’ll learn more about all these
    concepts in the next few chapters.
2.  Of course, the interactive help mode doesn’t know everything. If you
    type something that isn’t a Python command, module, function, or
    other built-in keyword, the interactive help mode will just shrug
    its virtual shoulders.
3.  To quit the interactive help mode, type quit and press ENTER.
4.  The prompt changes back to \>\>\> to signal that you’ve left the
    interactive help mode and returned to the Python Shell.

IDLE, the graphical Python Shell, also includes a Python-aware text
editor.

⁂

Python Editors and IDEs {#editors}
-----------------------

IDLE is not the only game in town when it comes to writing programs in
Python. While it’s useful to get started with learning the language
itself, many developers prefer other text editors or Integrated
Development Environments (IDEs). I won’t cover them here, but the Python
community maintains [a list of Python-aware
editors](http://wiki.python.org/moin/PythonEditors) that covers a wide
range of supported platforms and software licenses.

You might also want to check out the [list of Python-aware
IDEs](http://wiki.python.org/moin/IntegratedDevelopmentEnvironments),
although few of them support Python 3 yet. One that does is
[PyDev](http://pydev.sourceforge.net/), a plugin for
[Eclipse](http://eclipse.org/) that turns Eclipse into a full-fledged
Python IDE. Both Eclipse and PyDev are cross-platform and open source.

On the commercial front, there is ActiveState’s [Komodo
IDE](http://www.activestate.com/komodo/). It has per-user licensing, but
students can get a discount, and a free time-limited trial version is
available.

I’ve been programming in Python for nine years, and I edit my Python
programs in [GNU Emacs](http://www.gnu.org/software/emacs/) and debug
them in the command-line Python Shell. There’s no right or wrong way to
develop in Python. Find a way that works for you!

[☜](whats-new.html "back to “What’s New In Dive Into Python 3”")
[☞](your-first-python-program.html "onward to “Your First Python Program”")

© 2001–11 [Mark Pilgrim](about.html)
