Classes & Iterators - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#iterators) ‣

Difficulty level: ♦♦♦♢♢

Classes *&* Iterators
=====================

> ❝ East is East, and West is West, and never the twain shall meet. ❞\
> — [Rudyard Kipling](http://en.wikiquote.org/wiki/Rudyard_Kipling)

 

Diving In {#divingin}
---------

Iterators are the “secret sauce” of Python 3. They’re everywhere,
underlying everything, always just out of sight.
[Comprehensions](comprehensions.html) are just a simple form of
*iterators*. Generators are just a simple form of *iterators*. A
function that `yield`s values is a nice, compact way of building an
iterator without building an iterator. Let me show you what I mean by
that.

Remember [the Fibonacci
generator](generators.html#a-fibonacci-generator)? Here it is as a
built-from-scratch iterator:

[[download `fibonacci2.py`](examples/fibonacci2.py)]

~~~~ {.pp}
class Fib:
    '''iterator that yields numbers in the Fibonacci sequence'''

    def __init__(self, max):
        self.max = max

    def __iter__(self):
        self.a = 0
        self.b = 1
        return self

    def __next__(self):
        fib = self.a
        if fib > self.max:
            raise StopIteration
        self.a, self.b = self.b, self.a + self.b
        return fib
~~~~

Let’s take that one line at a time.

~~~~ {.nd .pp}
class Fib:
~~~~

`class`? What’s a class?

⁂

Defining Classes
----------------

Python is fully object-oriented: you can define your own classes,
inherit from your own or built-in classes, and instantiate the classes
you’ve defined.

Defining a class in Python is simple. As with functions, there is no
separate interface definition. Just define the class and start coding. A
Python class starts with the reserved word `class`, followed by the
class name. Technically, that’s all that’s required, since a class
doesn’t need to inherit from any other class.

~~~~ {.pp}
class PapayaWhip:  ①
    pass           ②
~~~~

1.  The name of this class is `PapayaWhip`, and it doesn’t inherit from
    any other class. Class names are usually capitalized,
    `EachWordLikeThis`, but this is only a convention, not a
    requirement.
2.  You probably guessed this, but everything in a class is indented,
    just like the code within a function, `if` statement, `for` loop, or
    any other block of code. The first line not indented is outside the
    class.

This `PapayaWhip` class doesn’t define any methods or attributes, but
syntactically, there needs to be something in the definition, thus the
`pass` statement. This is a Python reserved word that just means “move
along, nothing to see here”. It’s a statement that does nothing, and
it’s a good placeholder when you’re stubbing out functions or classes.

> ☞The `pass` statement in Python is like a empty set of curly braces
> (`{}`) in Java or C.

Many classes are inherited from other classes, but this one is not. Many
classes define methods, but this one does not. There is nothing that a
Python class absolutely must have, other than a name. In particular, C++
programmers may find it odd that Python classes don’t have explicit
constructors and destructors. Although it’s not required, Python classes
*can* have something similar to a constructor: the `__init__()` method.

### The `__init__()` Method {#init-method}

This example shows the initialization of the `Fib` class using the
`__init__` method.

~~~~ {.pp}
class Fib:
    '''iterator that yields numbers in the Fibonacci sequence'''  ①

    def __init__(self, max):                                      ②
~~~~

1.  Classes can (and should) have `docstring`s too, just like modules
    and functions.
2.  The `__init__()` method is called immediately after an instance of
    the class is created. It would be tempting — but technically
    incorrect — to call this the “constructor” of the class. It’s
    tempting, because it looks like a C++ constructor (by convention,
    the `__init__()` method is the first method defined for the class),
    acts like one (it’s the first piece of code executed in a newly
    created instance of the class), and even sounds like one. Incorrect,
    because the object has already been constructed by the time the
    `__init__()` method is called, and you already have a valid
    reference to the new instance of the class.

The first argument of every class method, including the `__init__()`
method, is always a reference to the current instance of the class. By
convention, this argument is named self. This argument fills the role of
the reserved word `this` in C++ or Java, but self is not a reserved word
in Python, merely a naming convention. Nonetheless, please don’t call it
anything but self; this is a very strong convention.

In all class methods, self refers to the instance whose method was
called. But in the specific case of the `__init__()` method, the
instance whose method was called is also the newly created object.
Although you need to specify self explicitly when defining the method,
you do *not* specify it when calling the method; Python will add it for
you automatically.

⁂

Instantiating Classes
---------------------

Instantiating classes in Python is straightforward. To instantiate a
class, simply call the class as if it were a function, passing the
arguments that the `__init__()` method requires. The return value will
be the newly created object.

~~~~ {.screen}
>>> import fibonacci2
>>> fib = fibonacci2.Fib(100)  ①
>>> fib                        ②
<fibonacci2.Fib object at 0x00DB8810>
>>> fib.__class__              ③
<class 'fibonacci2.Fib'>
>>> fib.__doc__                ④
'iterator that yields numbers in the Fibonacci sequence'
~~~~

1.  You are creating an instance of the `Fib` class (defined in the
    `fibonacci2` module) and assigning the newly created instance to the
    variable fib. You are passing one parameter, `100`, which will end
    up as the max argument in `Fib`’s `__init__()` method.
2.  fib is now an instance of the `Fib` class.
3.  Every class instance has a built-in attribute, `__class__`, which is
    the object’s class. Java programmers may be familiar with the
    `Class` class, which contains methods like `getName()` and
    `getSuperclass()` to get metadata information about an object. In
    Python, this kind of metadata is available through attributes, but
    the idea is the same.
4.  You can access the instance’s `docstring` just as with a function or
    a module. All instances of a class share the same `docstring`.

> ☞In Python, simply call a class as if it were a function to create a
> new instance of the class. There is no explicit `new` operator like
> there is in C++ or Java.

⁂

Instance Variables
------------------

On to the next line:

~~~~ {.pp}
class Fib:
    def __init__(self, max):
        self.max = max        ①
~~~~

1.  What is self.max? It’s an instance variable. It is completely
    separate from max, which was passed into the `__init__()` method as
    an argument. self.max is “global” to the instance. That means that
    you can access it from other methods.

~~~~ {.pp}
class Fib:
    def __init__(self, max):
        self.max = max        ①
    .
    .
    .
    def __next__(self):
        fib = self.a
        if fib > self.max:    ②
~~~~

1.  self.max is defined in the `__init__()` method…
2.  …and referenced in the `__next__()` method.

Instance variables are specific to one instance of a class. For example,
if you create two `Fib` instances with different maximum values, they
will each remember their own values.

~~~~ {.nd .screen}
>>> import fibonacci2
>>> fib1 = fibonacci2.Fib(100)
>>> fib2 = fibonacci2.Fib(200)
>>> fib1.max
100
>>> fib2.max
200
~~~~

⁂

A Fibonacci Iterator
--------------------

*Now* you’re ready to learn how to build an iterator. An iterator is
just a class that defines an `__iter__()` method.

All three of these class methods, `__init__`, `__iter__`, and
`__next__`, begin and end with a pair of underscore (`_`) characters.
Why is that? There’s nothing magical about it, but it usually indicates
that these are “special methods.” The only thing “special” about special
methods is that they aren’t called directly; Python calls them when you
use some other syntax on the class or an instance of the class. [More
about special methods](special-method-names.html).

[[download `fibonacci2.py`](examples/fibonacci2.py)]

~~~~ {.pp}
class Fib:                                        ①
    def __init__(self, max):                      ②
        self.max = max

    def __iter__(self):                           ③
        self.a = 0
        self.b = 1
        return self

    def __next__(self):                           ④
        fib = self.a
        if fib > self.max:
            raise StopIteration                   ⑤
        self.a, self.b = self.b, self.a + self.b
        return fib                                ⑥
~~~~

1.  To build an iterator from scratch, `Fib` needs to be a class, not a
    function.
2.  “Calling” `Fib(max)` is really creating an instance of this class
    and calling its `__init__()` method with max. The `__init__()`
    method saves the maximum value as an instance variable so other
    methods can refer to it later.
3.  The `__iter__()` method is called whenever someone calls
    `iter(fib)`. (As you’ll see in a minute, a `for` loop will call this
    automatically, but you can also call it yourself manually.) After
    performing beginning-of-iteration initialization (in this case,
    resetting `self.a` and `self.b`, our two counters), the `__iter__()`
    method can return any object that implements a `__next__()` method.
    In this case (and in most cases), `__iter__()` simply returns self,
    since this class implements its own `__next__()` method.
4.  The `__next__()` method is called whenever someone calls `next()` on
    an iterator of an instance of a class. That will make more sense in
    a minute.
5.  When the `__next__()` method raises a `StopIteration` exception,
    this signals to the caller that the iteration is exhausted. Unlike
    most exceptions, this is not an error; it’s a normal condition that
    just means that the iterator has no more values to generate. If the
    caller is a `for` loop, it will notice this `StopIteration`
    exception and gracefully exit the loop. (In other words, it will
    swallow the exception.) This little bit of magic is actually the key
    to using iterators in `for` loops.
6.  To spit out the next value, an iterator’s `__next__()` method simply
    `return`s the value. Do not use `yield` here; that’s a bit of
    syntactic sugar that only applies when you’re using generators. Here
    you’re creating your own iterator from scratch; use `return`
    instead.

Thoroughly confused yet? Excellent. Let’s see how to call this iterator:

~~~~ {.nd .screen}
>>> from fibonacci2 import Fib
>>> for n in Fib(1000):
...     print(n, end=' ')
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
~~~~

Why, it’s exactly the same! Byte for byte identical to how you called
[Fibonacci-as-a-generator](generators.html#a-fibonacci-generator)
(modulo one capital letter). But how?

There’s a bit of magic involved in `for` loops. Here’s what happens:

-   The `for` loop calls `Fib(1000)`, as shown. This returns an instance
    of the `Fib` class. Call this fib\_inst.
-   Secretly, and quite cleverly, the `for` loop calls `iter(fib_inst)`,
    which returns an iterator object. Call this fib\_iter. In this case,
    fib\_iter == fib\_inst, because the `__iter__()` method returns
    self, but the `for` loop doesn’t know (or care) about that.
-   To “loop through” the iterator, the `for` loop calls
    `next(fib_iter)`, which calls the `__next__()` method on the
    `fib_iter` object, which does the next-Fibonacci-number calculations
    and returns a value. The `for` loop takes this value and assigns it
    to n, then executes the body of the `for` loop for that value of n.
-   How does the `for` loop know when to stop? I’m glad you asked! When
    `next(fib_iter)` raises a `StopIteration` exception, the `for` loop
    will swallow the exception and gracefully exit. (Any other exception
    will pass through and be raised as usual.) And where have you seen a
    `StopIteration` exception? In the `__next__()` method, of course!

⁂

A Plural Rule Iterator
----------------------

iter(f) calls f.\_\_iter\_\_\
next(f) calls f.\_\_next\_\_

Now it’s time for the finale. Let’s rewrite the [plural rules
generator](generators.html) as an iterator.

[[download `plural6.py`](examples/plural6.py)]

~~~~ {.pp}
class LazyRules:
    rules_filename = 'plural6-rules.txt'

    def __init__(self):
        self.pattern_file = open(self.rules_filename, encoding='utf-8')
        self.cache = []

    def __iter__(self):
        self.cache_index = 0
        return self

    def __next__(self):
        self.cache_index += 1
        if len(self.cache) >= self.cache_index:
            return self.cache[self.cache_index - 1]

        if self.pattern_file.closed:
            raise StopIteration

        line = self.pattern_file.readline()
        if not line:
            self.pattern_file.close()
            raise StopIteration

        pattern, search, replace = line.split(None, 3)
        funcs = build_match_and_apply_functions(
            pattern, search, replace)
        self.cache.append(funcs)
        return funcs

rules = LazyRules()
~~~~

So this is a class that implements `__iter__()` and `__next__()`, so it
can be used as an iterator. Then, you instantiate the class and assign
it to rules. This happens just once, on import.

Let’s take the class one bite at a time.

~~~~ {.pp}
class LazyRules:
    rules_filename = 'plural6-rules.txt'

    def __init__(self):
        self.pattern_file = open(self.rules_filename, encoding='utf-8')  ①
        self.cache = []                                                  ②
~~~~

1.  When we instantiate the `LazyRules` class, open the pattern file but
    don’t read anything from it. (That comes later.)
2.  After opening the patterns file, initialize the cache. You’ll use
    this cache later (in the `__next__()` method) as you read lines from
    the pattern file.

Before we continue, let’s take a closer look at rules\_filename. It’s
not defined within the `__iter__()` method. In fact, it’s not defined
within *any* method. It’s defined at the class level. It’s a *class
variable*, and although you can access it just like an instance variable
(self.rules\_filename), it is shared across all instances of the
`LazyRules` class.

~~~~ {.screen}
>>> import plural6
>>> r1 = plural6.LazyRules()
>>> r2 = plural6.LazyRules()
>>> r1.rules_filename                               ①
'plural6-rules.txt'
>>> r2.rules_filename
'plural6-rules.txt'
>>> r2.rules_filename = 'r2-override.txt'           ②
>>> r2.rules_filename
'r2-override.txt'
>>> r1.rules_filename
'plural6-rules.txt'
>>> r2.__class__.rules_filename                     ③
'plural6-rules.txt'
>>> r2.__class__.rules_filename = 'papayawhip.txt'  ④
>>> r1.rules_filename
'papayawhip.txt'
>>> r2.rules_filename                               ⑤
'r2-overridetxt'
~~~~

1.  Each instance of the class inherits the rules\_filename attribute
    with the value defined by the class.
2.  Changing the attribute’s value in one instance does not affect other
    instances…
3.  …nor does it change the class attribute. You can access the class
    attribute (as opposed to an individual instance’s attribute) by
    using the special `__class__` attribute to access the class itself.
4.  If you change the class attribute, all instances that are still
    inheriting that value (like r1 here) will be affected.
5.  Instances that have overridden that attribute (like r2 here) will
    not be affected.

And now back to our show.

~~~~ {.pp}
    def __iter__(self):       ①
        self.cache_index = 0
        return self           ②
~~~~

1.  The `__iter__()` method will be called every time someone — say, a
    `for` loop — calls `iter(rules)`.
2.  The one thing that every `__iter__()` method must do is return an
    iterator. In this case, it returns self, which signals that this
    class defines a `__next__()` method which will take care of
    returning values throughout the iteration.

~~~~ {.pp}
    def __next__(self):                                 ①
        .
        .
        .
        pattern, search, replace = line.split(None, 3)
        funcs = build_match_and_apply_functions(        ②
            pattern, search, replace)
        self.cache.append(funcs)                        ③
        return funcs
~~~~

1.  The `__next__()` method gets called whenever someone — say, a `for`
    loop — calls `next(rules)`. This method will only make sense if we
    start at the end and work backwards. So let’s do that.
2.  The last part of this function should look familiar, at least. The
    `build_match_and_apply_functions()` function hasn’t changed; it’s
    the same as it ever was.
3.  The only difference is that, before returning the match and apply
    functions (which are stored in the tuple funcs), we’re going to save
    them in `self.cache`.

Moving backwards…

~~~~ {.pp}
    def __next__(self):
        .
        .
        .
        line = self.pattern_file.readline()  ①
        if not line:                         ②
            self.pattern_file.close()
            raise StopIteration              ③
        .
        .
        .
~~~~

1.  A bit of advanced file trickery here. The `readline()` method (note:
    singular, not the plural `readlines()`) reads exactly one line from
    an open file. Specifically, the next line. (*File objects are
    iterators too! It’s iterators all the way down…*)
2.  If there was a line for `readline()` to read, line will not be an
    empty string. Even if the file contained a blank line, line would
    end up as the one-character string `'\n'` (a carriage return). If
    line is really an empty string, that means there are no more lines
    to read from the file.
3.  When we reach the end of the file, we should close the file and
    raise the magic `StopIteration` exception. Remember, we got to this
    point because we needed a match and apply function for the next
    rule. The next rule comes from the next line of the file… but there
    is no next line! Therefore, we have no value to return. The
    iteration is over. (♫ The party’s over… ♫)

Moving backwards all the way to the start of the `__next__()` method…

~~~~ {.pp}
    def __next__(self):
        self.cache_index += 1
        if len(self.cache) >= self.cache_index:
            return self.cache[self.cache_index - 1]     ①

        if self.pattern_file.closed:
            raise StopIteration                         ②
        .
        .
        .
~~~~

1.  `self.cache` will be a list of the functions we need to match and
    apply individual rules. (At least *that* should sound familiar!)
    `self.cache_index` keeps track of which cached item we should return
    next. If we haven’t exhausted the cache yet (*i.e.* if the length of
    `self.cache` is greater than `self.cache_index`), then we have a
    cache hit! Hooray! We can return the match and apply functions from
    the cache instead of building them from scratch.
2.  On the other hand, if we don’t get a hit from the cache, *and* the
    file object has been closed (which could happen, further down the
    method, as you saw in the previous code snippet), then there’s
    nothing more we can do. If the file is closed, it means we’ve
    exhausted it — we’ve already read through every line from the
    pattern file, and we’ve already built and cached the match and apply
    functions for each pattern. The file is exhausted; the cache is
    exhausted; I’m exhausted. Wait, what? Hang in there, we’re almost
    done.

Putting it all together, here’s what happens when:

-   When the module is imported, it creates a single instance of the
    `LazyRules` class, called rules, which opens the pattern file but
    does not read from it.
-   When asked for the first match and apply function, it checks its
    cache but finds the cache is empty. So it reads a single line from
    the pattern file, builds the match and apply functions from those
    patterns, and caches them.
-   Let’s say, for the sake of argument, that the very first rule
    matched. If so, no further match and apply functions are built, and
    no further lines are read from the pattern file.
-   Furthermore, for the sake of argument, suppose that the caller calls
    the `plural()` function *again* to pluralize a different word. The
    `for` loop in the `plural()` function will call `iter(rules)`, which
    will reset the cache index but will not reset the open file object.
-   The first time through, the `for` loop will ask for a value from
    rules, which will invoke its `__next__()` method. This time,
    however, the cache is primed with a single pair of match and apply
    functions, corresponding to the patterns in the first line of the
    pattern file. Since they were built and cached in the course of
    pluralizing the previous word, they’re retrieved from the cache. The
    cache index increments, and the open file is never touched.
-   Let’s say, for the sake of argument, that the first rule does *not*
    match this time around. So the `for` loop comes around again and
    asks for another value from rules. This invokes the `__next__()`
    method a second time. This time, the cache is exhausted — it only
    contained one item, and we’re asking for a second — so the
    `__next__()` method continues. It reads another line from the open
    file, builds match and apply functions out of the patterns, and
    caches them.
-   This read-build-and-cache process will continue as long as the rules
    being read from the pattern file don’t match the word we’re trying
    to pluralize. If we do find a matching rule before the end of the
    file, we simply use it and stop, with the file still open. The file
    pointer will stay wherever we stopped reading, waiting for the next
    `readline()` command. In the meantime, the cache now has more items
    in it, and if we start all over again trying to pluralize a new
    word, each of those items in the cache will be tried before reading
    the next line from the pattern file.

We have achieved pluralization nirvana.

1.  **Minimal startup cost.** The only thing that happens on `import` is
    instantiating a single class and opening a file (but not reading
    from it).
2.  **Maximum performance.** The previous example would read through the
    file and build functions dynamically every time you wanted to
    pluralize a word. This version will cache functions as soon as
    they’re built, and in the worst case, it will only read through the
    pattern file once, no matter how many words you pluralize.
3.  **Separation of code and data.** All the patterns are stored in a
    separate file. Code is code, and data is data, and never the twain
    shall meet.

> ☞Is this really nirvana? Well, yes and no. Here’s something to
> consider with the `LazyRules` example: the pattern file is opened
> (during `__init__()`), and it remains open until the final rule is
> reached. Python will eventually close the file when it exits, or after
> the last instantiation of the `LazyRules` class is destroyed, but
> still, that could be a *long* time. If this class is part of a
> long-running Python process, the Python interpreter may never exit,
> and the `LazyRules` object may never get destroyed.
>
> There are ways around this. Instead of opening the file during
> `__init__()` and leaving it open while you read rules one line at a
> time, you could open the file, read all the rules, and immediately
> close the file. Or you could open the file, read one rule, save the
> file position with the [`tell()` method](files.html#read), close the
> file, and later re-open it and use the [`seek()`
> method](files.html#read) to continue reading where you left off. Or
> you could not worry about it and just leave the file open, like this
> example code does. Programming is design, and design is all about
> trade-offs and constraints. Leaving a file open too long might be a
> problem; making your code more complicated might be a problem. Which
> one is the bigger problem depends on your development team, your
> application, and your runtime environment.

⁂

Further Reading {#furtherreading}
---------------

-   [Iterator
    types](http://docs.python.org/3.1/library/stdtypes.html#iterator-types)
-   [PEP 234: Iterators](http://www.python.org/dev/peps/pep-0234/)
-   [PEP 255: Simple
    Generators](http://www.python.org/dev/peps/pep-0255/)
-   [Generator Tricks for Systems
    Programmers](http://www.dabeaz.com/generators/)

[☜](generators.html "back to “Closures & Generators”")
[☞](advanced-iterators.html "onward to “Advanced Iterators”")

© 2001–11 [Mark Pilgrim](about.html)
