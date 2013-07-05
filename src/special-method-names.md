Special Method Names - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#special-method-names) ‣

Difficulty level: ♦♦♦♦♦

Special Method Names
====================

> ❝ My specialty is being right when other people are wrong. ❞\
> — [George Bernard
> Shaw](http://en.wikiquote.org/wiki/George_Bernard_Shaw)

 

Diving In {#divingin}
---------

Throughout this book, you’ve seen examples of “special
methods” — certain “magic” methods that Python invokes when you use
certain syntax. Using special methods, your classes can act like sets,
like dictionaries, like functions, like iterators, or even like numbers.
This appendix serves both as a reference for the special methods we’ve
seen already and a brief introduction to some of the more esoteric ones.

Basics
------

If you’ve read the [introduction to classes](iterators.html#divingin),
you’ve already seen the most common special method: the `__init__()`
method. The majority of classes I write end up needing some
initialization. There are also a few other basic special methods that
are especially useful for debugging your custom classes.

Notes

You Want…

So You Write…

And Python Calls…

①

to initialize an instance

`x = MyClass()`{.pp}

[`x.__init__()`](http://docs.python.org/3.1/reference/datamodel.html#object.__init__)

②

the “official” representation as a string

`repr(x)`{.pp}

[`x.__repr__()`](http://docs.python.org/3.1/reference/datamodel.html#object.__repr__)

③

the “informal” value as a string

[`str(x)`](http://docs.python.org/3.1/reference/datamodel.html#object.__str__)

`x.__str__()`{.pp}

④

the “informal” value as a byte array

`bytes(x)`{.pp}

`x.__bytes__()`{.pp}

⑤

the value as a formatted string

`format(x, format_spec)`{.pp}

[`x.__format__(format_spec)`](http://docs.python.org/3.1/reference/datamodel.html#object.__format__)

1.  The `__init__()` method is called *after* the instance is created.
    If you want to control the actual creation process, use [the
    `__new__()` method](#esoterica).
2.  By convention, the `__repr__()` method should return a string that
    is a valid Python expression.
3.  The `__str__()` method is also called when you `print(x)`.
4.  *New in Python 3*, since the `bytes` type was introduced.
5.  By convention, format\_spec should conform to the [Format
    Specification
    Mini-Language](http://www.python.org/doc/3.1/library/string.html#formatspec).
    `decimal.py` in the Python standard library provides its own
    `__format__()` method.

Classes That Act Like Iterators {#acts-like-iterator}
-------------------------------

In [the Iterators chapter](iterators.html), you saw how to build an
iterator from the ground up using the `__iter__()` and `__next__()`
methods.

Notes

You Want…

So You Write…

And Python Calls…

①

to iterate through a sequence

`iter(seq)`{.pp}

[`seq.__iter__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__iter__)

②

to get the next value from an iterator

`next(seq)`{.pp}

[`seq.__next__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__next__)

③

to create an iterator in reverse order

`reversed(seq)`{.pp}

[`seq.__reversed__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__reversed__)

1.  The `__iter__()` method is called whenever you create a new
    iterator. It’s a good place to initialize the iterator with initial
    values.
2.  The `__next__()` method is called whenever you retrieve the next
    value from an iterator.
3.  The `__reversed__()` method is uncommon. It takes an existing
    sequence and returns an iterator that yields the items in the
    sequence in reverse order, from last to first.

As you saw in [the Iterators
chapter](iterators.html#a-fibonacci-iterator), a `for` loop can act on
an iterator. In this loop:

~~~~ {.nd .pp}
for x in seq:
    print(x)
~~~~

Python 3 will call `seq.__iter__()` to create an iterator, then call the
`__next__()` method on that iterator to get each value of x. When the
`__next__()` method raises a `StopIteration` exception, the `for` loop
ends gracefully.

Computed Attributes
-------------------

Notes

You Want…

So You Write…

And Python Calls…

①

to get a computed attribute (unconditionally)

`x.my_property`{.pp}

[`x.__getattribute__('my_property')`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__getattribute__)

②

to get a computed attribute (fallback)

`x.my_property`{.pp}

[`x.__getattr__('my_property')`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__getattr__)

③

to set an attribute

`x.my_property = value`{.pp}

[`x.__setattr__('my_property', value)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__setattr__)

④

to delete an attribute

`del x.my_property`{.pp}

[`x.__delattr__('my_property')`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__delattr__)

⑤

to list all attributes and methods

`dir(x)`{.pp}

[`x.__dir__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__dir__)

1.  If your class defines a `__getattribute__()` method, Python will
    call it on *every reference to any attribute or method name* (except
    special method names, since that would cause an unpleasant infinite
    loop).
2.  If your class defines a `__getattr__()` method, Python will call it
    only after looking for the attribute in all the normal places. If an
    instance x defines an attribute color, `x.color` will *not* call
    `x.__getattr__('color')`; it will simply return the already-defined
    value of x.color.
3.  The `__setattr__()` method is called whenever you assign a value to
    an attribute.
4.  The `__delattr__()` method is called whenever you delete an
    attribute.
5.  The `__dir__()` method is useful if you define a `__getattr__()` or
    `__getattribute__()` method. Normally, calling `dir(x)` would only
    list the regular attributes and methods. If your `__getattr__()`
    method handles a color attribute dynamically, `dir(x)` would not
    list color as one of the available attributes. Overriding the
    `__dir__()` method allows you to list color as an available
    attribute, which is helpful for other people who wish to use your
    class without digging into the internals of it.

The distinction between the `__getattr__()` and `__getattribute__()`
methods is subtle but important. I can explain it with two examples:

~~~~ {.screen}
class Dynamo:
    def __getattr__(self, key):
        if key == 'color':         ①
            return 'PapayaWhip'
        else:
            raise AttributeError   ②

>>> dyn = Dynamo()
>>> dyn.color                      ③
'PapayaWhip'
>>> dyn.color = 'LemonChiffon'
>>> dyn.color                      ④
'LemonChiffon'
~~~~

1.  The attribute name is passed into the `__getattr__()` method as a
    string. If the name is `'color'`, the method returns a value. (In
    this case, it’s just a hard-coded string, but you would normally do
    some sort of computation and return the result.)
2.  If the attribute name is unknown, the `__getattr__()` method needs
    to raise an `AttributeError` exception, otherwise your code will
    silently fail when accessing undefined attributes. (Technically, if
    the method doesn’t raise an exception or explicitly return a value,
    it returns `None`, the Python null value. This means that *all*
    attributes not explicitly defined will be `None`, which is almost
    certainly not what you want.)
3.  The dyn instance does not have an attribute named color, so the
    `__getattr__()` method is called to provide a computed value.
4.  After explicitly setting dyn.color, the `__getattr__()` method will
    no longer be called to provide a value for dyn.color, because
    dyn.color is already defined on the instance.

On the other hand, the `__getattribute__()` method is absolute and
unconditional.

~~~~ {.screen}
class SuperDynamo:
    def __getattribute__(self, key):
        if key == 'color':
            return 'PapayaWhip'
        else:
            raise AttributeError

>>> dyn = SuperDynamo()
>>> dyn.color                      ①
'PapayaWhip'
>>> dyn.color = 'LemonChiffon'
>>> dyn.color                      ②
'PapayaWhip'
~~~~

1.  The `__getattribute__()` method is called to provide a value for
    dyn.color.
2.  Even after explicitly setting dyn.color, the `__getattribute__()`
    method *is still called* to provide a value for dyn.color. If
    present, the `__getattribute__()` method *is called unconditionally*
    for every attribute and method lookup, even for attributes that you
    explicitly set after creating an instance.

> ☞If your class defines a `__getattribute__()` method, you probably
> also want to define a `__setattr__()` method and coordinate between
> them to keep track of attribute values. Otherwise, any attributes you
> set after creating an instance will disappear into a black hole.

You need to be extra careful with the `__getattribute__()` method,
because it is also called when Python looks up a method name on your
class.

~~~~ {.screen}
class Rastan:
    def __getattribute__(self, key):
        raise AttributeError           ①
    def swim(self):
        pass

>>> hero = Rastan()
>>> hero.swim()                        ②
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in __getattribute__
AttributeError
~~~~

1.  This class defines a `__getattribute__()` method which always raises
    an `AttributeError` exception. No attribute or method lookups will
    succeed.
2.  When you call `hero.swim()`, Python looks for a `swim()` method in
    the `Rastan` class. This lookup goes through the
    `__getattribute__()` method, *because all attribute and method
    lookups go through the `__getattribute__()` method*. In this case,
    the `__getattribute__()` method raises an `AttributeError`
    exception, so the method lookup fails, so the method call fails.

Classes That Act Like Functions {#acts-like-function}
-------------------------------

You can make an instance of a class callable — exactly like a function
is callable — by defining the `__call__()` method.

Notes

You Want…

So You Write…

And Python Calls…

to “call” an instance like a function

`my_instance()`{.pp}

[`my_instance.__call__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__call__)

The [`zipfile` module](http://docs.python.org/3.1/library/zipfile.html)
uses this to define a class that can decrypt an encrypted zip file with
a given password. The zip decryption algorithm requires you to store
state during decryption. Defining the decryptor as a class allows you to
maintain this state within a single instance of the decryptor class. The
state is initialized in the `__init__()` method and updated as the file
is decrypted. But since the class is also “callable” like a function,
you can pass the instance as the first argument of the `map()` function,
like so:

~~~~ {.pp}
# excerpt from zipfile.py
class _ZipDecrypter:
.
.
.
    def __init__(self, pwd):
        self.key0 = 305419896               ①
        self.key1 = 591751049
        self.key2 = 878082192
        for p in pwd:
            self._UpdateKeys(p)

    def __call__(self, c):                  ②
        assert isinstance(c, int)
        k = self.key2 | 2
        c = c ^ (((k * (k^1)) >> 8) & 255)
        self._UpdateKeys(c)
        return c
.
.
.
zd = _ZipDecrypter(pwd)                    ③
bytes = zef_file.read(12)
h = list(map(zd, bytes[0:12]))             ④
~~~~

1.  The `_ZipDecryptor` class maintains state in the form of three
    rotating keys, which are later updated in the `_UpdateKeys()` method
    (not shown here).
2.  The class defines a `__call__()` method, which makes class instances
    callable like functions. In this case, the `__call__()` method
    decrypts a single byte of the zip file, then updates the rotating
    keys based on the byte that was decrypted.
3.  zd is an instance of the `_ZipDecryptor` class. The pwd variable is
    passed to the `__init__()` method, where it is stored and used to
    update the rotating keys for the first time.
4.  Given the first 12 bytes of a zip file, decrypt them by mapping the
    bytes to zd, in effect “calling” zd 12 times, which invokes the
    `__call__()` method 12 times, which updates its internal state and
    returns a resulting byte 12 times.

Classes That Act Like Sets {#acts-like-set}
--------------------------

If your class acts as a container for a set of values — that is, if it
makes sense to ask whether your class “contains” a value — then it
should probably define the following special methods that make it act
like a set.

Notes

You Want…

So You Write…

And Python Calls…

the number of items

`len(s)`{.pp}

[`s.__len__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__len__)

to know whether it contains a specific value

`x in s`{.pp}

[`s.__contains__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__contains__)

The [`cgi` module](http://docs.python.org/3.1/library/cgi.html) uses
these methods in its `FieldStorage` class, which represents all of the
form fields or query parameters submitted to a dynamic web page.

~~~~ {.pp}
# A script which responds to http://example.com/search?q=cgi
import cgi
fs = cgi.FieldStorage()
if 'q' in fs:                                               ①
  do_search()

# An excerpt from cgi.py that explains how that works
class FieldStorage:
.
.
.
    def __contains__(self, key):                            ②
        if self.list is None:
            raise TypeError('not indexable')
        return any(item.name == key for item in self.list)  ③

    def __len__(self):                                      ④
        return len(self.keys())                             ⑤
~~~~

1.  Once you create an instance of the `cgi.FieldStorage` class, you can
    use the “`in`” operator to check whether a particular parameter was
    included in the query string.
2.  The `__contains__()` method is the magic that makes this work. When
    you say `if 'q' in fs`, Python looks for the `__contains__()` method
    on the fs object, which is defined in `cgi.py`. The value `'q'` is
    passed into the `__contains__()` method as the key argument.
3.  The `any()` function takes a [generator
    expression](advanced-iterators.html#generator-expressions) and
    returns `True` if the generator spits out any items. The `any()`
    function is smart enough to stop as soon as the first match is
    found.
4.  The same `FieldStorage` class also supports returning its length, so
    you can say `len(fs)` and it will call the `__len__()` method on the
    `FieldStorage` class to return the number of query parameters that
    it identified.
5.  The `self.keys()` method checks whether `self.list is None`, so the
    `__len__` method doesn’t need to duplicate this error checking.

Classes That Act Like Dictionaries {#acts-like-dict}
----------------------------------

Extending the previous section a bit, you can define classes that not
only respond to the “`in`” operator and the `len()` function, but they
act like full-blown dictionaries, returning values based on keys.

Notes

You Want…

So You Write…

And Python Calls…

to get a value by its key

`x[key]`{.pp}

[`x.__getitem__(key)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__getitem__)

to set a value by its key

`x[key] = value`{.pp}

[`x.__setitem__(key, value)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__setitem__)

to delete a key-value pair

`del x[key]`{.pp}

[`x.__delitem__(key)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__delitem__)

to provide a default value for missing keys

`x[nonexistent_key]`{.pp}

[`x.__missing__(nonexistent_key)`](http://docs.python.org/3.1/library/collections.html#collections.defaultdict.__missing__)

The [`FieldStorage` class](#acts-like-set-example) from the [`cgi`
module](http://docs.python.org/3.1/library/cgi.html) also defines these
special methods, which means you can do things like this:

~~~~ {.pp}
# A script which responds to http://example.com/search?q=cgi
import cgi
fs = cgi.FieldStorage()
if 'q' in fs:
  do_search(fs['q'])                              ①

# An excerpt from cgi.py that shows how it works
class FieldStorage:
.
.
.
    def __getitem__(self, key):                   ②
        if self.list is None:
            raise TypeError('not indexable')
        found = []
        for item in self.list:
            if item.name == key: found.append(item)
        if not found:
            raise KeyError(key)
        if len(found) == 1:
            return found[0]
        else:
            return found
~~~~

1.  The fs object is an instance of `cgi.FieldStorage`, but you can
    still evaluate expressions like `fs['q']`.
2.  `fs['q']` invokes the `__getitem__()` method with the key parameter
    set to `'q'`. It then looks up in its internally maintained list of
    query parameters (self.list) for an item whose `.name` matches the
    given key.

Classes That Act Like Numbers {#acts-like-number}
-----------------------------

Using the appropriate special methods, you can define your own classes
that act like numbers. That is, you can add them, subtract them, and
perform other mathematical operations on them. This is how fractions are
implemented — the `Fraction` class implements these special methods,
then you can do things like this:

~~~~ {.screen}
>>> from fractions import Fraction
>>> x = Fraction(1, 3)
>>> x / 3
Fraction(1, 9)
~~~~

Here is the comprehensive list of special methods you need to implement
a number-like class.

Notes

You Want…

So You Write…

And Python Calls…

addition

`x + y`{.pp}

[`x.__add__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__add__)

subtraction

`x - y`{.pp}

[`x.__sub__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__sub__)

multiplication

`x * y`{.pp}

[`x.__mul__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__mul__)

division

`x / y`{.pp}

[`x.__truediv__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__truediv__)

floor division

`x // y`

[`x.__floordiv__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__floordiv__)

modulo (remainder)

`x % y`{.pp}

[`x.__mod__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__mod__)

floor division *&* modulo

`divmod(x, y)`{.pp}

[`x.__divmod__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__divmod__)

raise to power

`x ** y`{.pp}

[`x.__pow__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__pow__)

left bit-shift

`x << y`{.pp}

[`x.__lshift__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__lshift__)

right bit-shift

`x >> y`{.pp}

[`x.__rshift__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rshift__)

bitwise `and`

`x & y`{.pp}

[`x.__and__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__and__)

bitwise `xor`

`x ^ y`{.pp}

[`x.__xor__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__xor__)

bitwise `or`

`x | y`{.pp}

[`x.__or__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__or__)

That’s all well and good if x is an instance of a class that implements
those methods. But what if it doesn’t implement one of them? Or worse,
what if it implements it, but it can’t handle certain kinds of
arguments? For example:

~~~~ {.screen}
>>> from fractions import Fraction
>>> x = Fraction(1, 3)
>>> 1 / x
Fraction(3, 1)
~~~~

This is *not* a case of taking a `Fraction` and dividing it by an
integer (as in the previous example). That case was straightforward:
`x / 3` calls `x.__truediv__(3)`, and the `__truediv__()` method of the
`Fraction` class handles all the math. But integers don’t “know” how to
do arithmetic operations with fractions. So why does this example work?

There is a second set of arithmetic special methods with *reflected
operands*. Given an arithmetic operation that takes two operands (*e.g.*
`x / y`), there are two ways to go about it:

1.  Tell x to divide itself by y, or
2.  Tell y to divide itself into x

The set of special methods above take the first approach: given `x / y`,
they provide a way for x to say “I know how to divide myself by y.” The
following set of special methods tackle the second approach: they
provide a way for y to say “I know how to be the denominator and divide
myself into x.”

Notes

You Want…

So You Write…

And Python Calls…

addition

`x + y`{.pp}

[`y.__radd__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__radd__)

subtraction

`x - y`{.pp}

[`y.__rsub__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rsub__)

multiplication

`x * y`{.pp}

[`y.__rmul__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rmul__)

division

`x / y`{.pp}

[`y.__rtruediv__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rtruediv__)

floor division

`x // y`

[`y.__rfloordiv__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rfloordiv__)

modulo (remainder)

`x % y`{.pp}

[`y.__rmod__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rmod__)

floor division *&* modulo

`divmod(x, y)`{.pp}

[`y.__rdivmod__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rdivmod__)

raise to power

`x ** y`{.pp}

[`y.__rpow__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rpow__)

left bit-shift

`x << y`{.pp}

[`y.__rlshift__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rlshift__)

right bit-shift

`x >> y`{.pp}

[`y.__rrshift__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rrshift__)

bitwise `and`

`x & y`{.pp}

[`y.__rand__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rand__)

bitwise `xor`

`x ^ y`{.pp}

[`y.__rxor__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__rxor__)

bitwise `or`

`x | y`{.pp}

[`y.__ror__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ror__)

But wait! There’s more! If you’re doing “in-place” operations, like
`x /= 3`, there are even more special methods you can define.

Notes

You Want…

So You Write…

And Python Calls…

in-place addition

`x += y`{.pp}

[`x.__iadd__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__iadd__)

in-place subtraction

`x -= y`{.pp}

[`x.__isub__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__isub__)

in-place multiplication

`x *= y`{.pp}

[`x.__imul__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__imul__)

in-place division

`x /= y`{.pp}

[`x.__itruediv__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__itruediv__)

in-place floor division

`x //= y`

[`x.__ifloordiv__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ifloordiv__)

in-place modulo

`x %= y`{.pp}

[`x.__imod__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__imod__)

in-place raise to power

`x **= y`{.pp}

[`x.__ipow__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ipow__)

in-place left bit-shift

`x <<= y`{.pp}

[`x.__ilshift__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ilshift__)

in-place right bit-shift

`x >>= y`{.pp}

[`x.__irshift__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__irshift__)

in-place bitwise `and`

`x &= y`{.pp}

[`x.__iand__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__iand__)

in-place bitwise `xor`

`x ^= y`{.pp}

[`x.__ixor__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ixor__)

in-place bitwise `or`

`x |= y`{.pp}

[`x.__ior__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ior__)

Note: for the most part, the in-place operation methods are not
required. If you don’t define an in-place method for a particular
operation, Python will try the methods. For example, to execute the
expression `x /= y`, Python will:

1.  Try calling `x.__itruediv__(y)`. If this method is defined and
    returns a value other than `NotImplemented`, we’re done.
2.  Try calling `x.__truediv__(y)`. If this method is defined and
    returns a value other than `NotImplemented`, the old value of x is
    discarded and replaced with the return value, just as if you had
    done ` x = x / y` instead.
3.  Try calling `y.__rtruediv__(x)`. If this method is defined and
    returns a value other than `NotImplemented`, the old value of x is
    discarded and replaced with the return value.

So you only need to define in-place methods like the `__itruediv__()`
method if you want to do some special optimization for in-place
operands. Otherwise Python will essentially reformulate the in-place
operand to use a regular operand + a variable assignment.

There are also a few “unary” mathematical operations you can perform on
number-like objects by themselves.

Notes

You Want…

So You Write…

And Python Calls…

negative number

`-x`{.pp}

[`x.__neg__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__neg__)

positive number

`+x`{.pp}

[`x.__pos__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__pos__)

absolute value

`abs(x)`{.pp}

[`x.__abs__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__abs__)

inverse

`~x`{.pp}

[`x.__invert__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__invert__)

complex number

`complex(x)`{.pp}

[`x.__complex__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__complex__)

integer

`int(x)`{.pp}

[`x.__int__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__int__)

floating point number

`float(x)`{.pp}

[`x.__float__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__float__)

number rounded to nearest integer

`round(x)`{.pp}

[`x.__round__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__round__)

number rounded to nearest n digits

`round(x, n)`{.pp}

[`x.__round__(n)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__round__)

smallest integer `>= x`

`math.ceil(x)`{.pp}

[`x.__ceil__()`](http://docs.python.org/3.1/library/math.html#math.ceil)

largest integer `<= x`

`math.floor(x)`{.pp}

[`x.__floor__()`](http://docs.python.org/3.1/library/math.html#math.floor)

truncate `x` to nearest integer toward 0

`math.trunc(x)`{.pp}

[`x.__trunc__()`](http://docs.python.org/3.1/library/math.html#math.trunc)

[PEP 357](http://www.python.org/dev/peps/pep-0357/)

number as a list index

`a_list[x]`{.pp}

[`a_list[x.__index__()]`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__index__)

Classes That Can Be Compared {#rich-comparisons}
----------------------------

I broke this section out from the previous one because comparisons are
not strictly the purview of numbers. Many datatypes can be
compared — strings, lists, even dictionaries. If you’re creating your
own class and it makes sense to compare your objects to other objects,
you can use the following special methods to implement comparisons.

Notes

You Want…

So You Write…

And Python Calls…

equality

`x == y`{.pp}

[`x.__eq__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__eq__)

inequality

`x != y`{.pp}

[`x.__ne__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ne__)

less than

`x < y`{.pp}

[`x.__lt__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__lt__)

less than or equal to

`x <= y`{.pp}

[`x.__le__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__le__)

greater than

`x >  y`{.pp}

[`x.__gt__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__gt__)

greater than or equal to

`x >= y`{.pp}

[`x.__ge__(y)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__ge__)

truth value in a boolean context

`if x:`{.pp}

[`x.__bool__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__bool__)

> ☞If you define a `__lt__()` method but no `__gt__()` method, Python
> will use the `__lt__()` method with operands swapped. However, Python
> will not combine methods. For example, if you define a `__lt__()`
> method and a `__eq__()` method and try to test whether `x <= y`,
> Python will not call `__lt__()` and `__eq__()` in sequence. It will
> only call the `__le__()` method.

Classes That Can Be Serialized {#pickle}
------------------------------

Python supports [serializing and unserializing arbitrary
objects](serializing.html). (Most Python references call this process
“pickling” and “unpickling.”) This can be useful for saving state to a
file and restoring it later. All of the [native
datatypes](native-datatypes.html) support pickling already. If you
create a custom class that you want to be able to pickle, read up on
[the pickle protocol](http://docs.python.org/3.1/library/pickle.html) to
see when and how the following special methods are called.

Notes

You Want…

So You Write…

And Python Calls…

a custom object copy

`copy.copy(x)`{.pp}

[`x.__copy__()`](http://docs.python.org/3.1/library/copy.html)

a custom object deepcopy

`copy.deepcopy(x)`{.pp}

[`x.__deepcopy__()`](http://docs.python.org/3.1/library/copy.html)

\*

to get an object’s state before pickling

`pickle.dump(x, file)`{.pp}

[`x.__getstate__()`](http://docs.python.org/3.1/library/pickle.html#pickle-state)

\*

to serialize an object

`pickle.dump(x, file)`{.pp}

[`x.__reduce__()`](http://docs.python.org/3.1/library/pickle.html#pickling-class-instances)

\*

to serialize an object (new pickling protocol)

`pickle.dump(x, file, protocol_version)`{.pp}

[`x.__reduce_ex__(protocol_version)`](http://docs.python.org/3.1/library/pickle.html#pickling-class-instances)

\*

control over how an object is created during unpickling

`x = pickle.load(file)`{.pp}

[`x.__getnewargs__()`](http://docs.python.org/3.1/library/pickle.html#pickling-class-instances)

\*

to restore an object’s state after unpickling

`x = pickle.load(file)`{.pp}

[`x.__setstate__()`](http://docs.python.org/3.1/library/pickle.html#pickle-state)

\* To recreate a serialized object, Python needs to create a new object
that looks like the serialized object, then set the values of all the
attributes on the new object. The `__getnewargs__()` method controls how
the object is created, then the `__setstate__()` method controls how the
attribute values are restored.

Classes That Can Be Used in a `with` Block {#context-managers}
------------------------------------------

A `with` block defines a [runtime
context](http://www.python.org/doc/3.1/library/stdtypes.html#typecontextmanager);
you “enter” the context when you execute the `with` statement, and you
“exit” the context after you execute the last statement in the block.

Notes

You Want…

So You Write…

And Python Calls…

do something special when entering a `with` block

`with x:`{.pp}

[`x.__enter__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__enter__)

do something special when leaving a `with` block

`with x:`{.pp}

[`x.__exit__(exc_type, exc_value, traceback)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__exit__)

This is how the [`with file` idiom](files.html#with) works.

~~~~ {.pp}
# excerpt from io.py:
def _checkClosed(self, msg=None):
    '''Internal: raise an ValueError if file is closed
    '''
    if self.closed:
        raise ValueError('I/O operation on closed file.'
                         if msg is None else msg)

def __enter__(self):
    '''Context management protocol.  Returns self.'''
    self._checkClosed()                                ①
    return self                                        ②

def __exit__(self, *args):
    '''Context management protocol.  Calls close()'''
    self.close()                                       ③
~~~~

1.  The file object defines both an `__enter__()` and an `__exit__()`
    method. The `__enter__()` method checks that the file is open; if
    it’s not, the `_checkClosed()` method raises an exception.
2.  The `__enter__()` method should almost always return self — this is
    the object that the `with` block will use to dispatch properties and
    methods.
3.  After the `with` block, the file object automatically closes. How?
    In the `__exit__()` method, it calls `self.close()`.

> ☞The `__exit__()` method will always be called, even if an exception
> is raised inside the `with` block. In fact, if an exception is raised,
> the exception information will be passed to the `__exit__()` method.
> See [With Statement Context
> Managers](http://www.python.org/doc/3.1/reference/datamodel.html#with-statement-context-managers)
> for more details.

For more on context managers, see [Closing Files
Automatically](files.html#with) and [Redirecting Standard
Output](files.html#redirect).

Really Esoteric Stuff {#esoterica}
---------------------

If you know what you’re doing, you can gain almost complete control over
how classes are compared, how attributes are defined, and what kinds of
classes are considered subclasses of your class.

Notes

You Want…

So You Write…

And Python Calls…

a class constructor

`x = MyClass()`{.pp}

[`x.__new__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__new__)

\*

a class destructor

`del x`{.pp}

[`x.__del__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__del__)

only a specific set of attributes to be defined

[`x.__slots__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__slots__)

a custom hash value

`hash(x)`{.pp}

[`x.__hash__()`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__hash__)

to get a property’s value

`x.color`{.pp}

[`type(x).__dict__['color'].__get__(x, type(x))`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__get__)

to set a property’s value

`x.color = 'PapayaWhip'`{.pp}

[`type(x).__dict__['color'].__set__(x, 'PapayaWhip')`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__set__)

to delete a property

`del x.color`{.pp}

[`type(x).__dict__['color'].__del__(x)`](http://www.python.org/doc/3.1/reference/datamodel.html#object.__delete__)

to control whether an object is an instance of your class

`isinstance(x, MyClass)`{.pp}

[`MyClass.__instancecheck__(x)`](http://www.python.org/dev/peps/pep-3119/#overloading-isinstance-and-issubclass)

to control whether a class is a subclass of your class

`issubclass(C, MyClass)`{.pp}

[`MyClass.__subclasscheck__(C)`](http://www.python.org/dev/peps/pep-3119/#overloading-isinstance-and-issubclass)

to control whether a class is a subclass of your abstract base class

`issubclass(C, MyABC)`{.pp}

[`MyABC.__subclasshook__(C)`](http://docs.python.org/3.1/library/abc.html#abc.ABCMeta.__subclasshook__)

^\*^ Exactly when Python calls the `__del__()` special method [is
incredibly
complicated](http://www.python.org/doc/3.1/reference/datamodel.html#object.__del__).
To fully understand it, you need to know how [Python keeps track of
objects in
memory](http://www.python.org/doc/3.1/reference/datamodel.html#objects-values-and-types).
Here’s a good article on [Python garbage collection and class
destructors](http://www.electricmonk.nl/log/2008/07/07/python-destructor-and-garbage-collection-notes/).
You should also read about [weak
references](http://mindtrove.info/articles/python-weak-references/), the
[`weakref` module](http://docs.python.org/3.1/library/weakref.html), and
probably the [`gc`
module](http://www.python.org/doc/3.1/library/gc.html) for good measure.

Further Reading {#furtherreading}
---------------

Modules mentioned in this appendix:

-   [`zipfile` module](http://docs.python.org/3.1/library/zipfile.html)
-   [`cgi` module](http://docs.python.org/3.1/library/cgi.html)
-   [`collections`
    module](http://www.python.org/doc/3.1/library/collections.html)
-   [`math` module](http://docs.python.org/3.1/library/math.html)
-   [`pickle` module](http://docs.python.org/3.1/library/pickle.html)
-   [`copy` module](http://docs.python.org/3.1/library/copy.html)
-   [`abc` (“Abstract Base Classes”)
    module](http://docs.python.org/3.1/library/abc.html)

Other light reading:

-   [Format Specification
    Mini-Language](http://www.python.org/doc/3.1/library/string.html#formatspec)
-   [Python data
    model](http://www.python.org/doc/3.1/reference/datamodel.html)
-   [Built-in
    types](http://www.python.org/doc/3.1/library/stdtypes.html)
-   [PEP 357: Allowing Any Object to be Used for
    Slicing](http://www.python.org/dev/peps/pep-0357/)
-   [PEP 3119: Introducing Abstract Base
    Classes](http://www.python.org/dev/peps/pep-3119/)

[☜](porting-code-to-python-3-with-2to3.html "back to “Porting code to Python 3 with 2to3”")
[☞](where-to-go-from-here.html "onward to “Where To Go From Here”")

© 2001–11 [Mark Pilgrim](about.html)
