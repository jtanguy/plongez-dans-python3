Closures & Generators - Dive Into Python 3

  

You are here: [Home](index.html) ‣ [Dive Into Python
3](table-of-contents.html#generators) ‣

Difficulty level: ♦♦♦♢♢

Closures *&* Generators
=======================

> ❝ My spelling is Wobbly. It’s good spelling but it Wobbles, and the
> letters get in the wrong places. ❞\
> — Winnie-the-Pooh

 

Diving In {#divingin}
---------

Having grown up the son of a librarian and an English major, I have
always been fascinated by languages. Not programming languages. Well
yes, programming languages, but also natural languages. Take English.
English is a schizophrenic language that borrows words from German,
French, Spanish, and Latin (to name a few). Actually, “borrows” is the
wrong word; “pillages” is more like it. Or perhaps “assimilates” — like
the Borg. Yes, I like that.

`We are the Borg. Your linguistic and etymological distinctiveness will be added to our own. Resistance is futile.`

In this chapter, you’re going to learn about plural nouns. Also,
functions that return other functions, advanced regular expressions, and
generators. But first, let’s talk about how to make plural nouns. (If
you haven’t read [the chapter on regular
expressions](regular-expressions.html), now would be a good time. This
chapter assumes you understand the basics of regular expressions, and it
quickly descends into more advanced uses.)

If you grew up in an English-speaking country or learned English in a
formal school setting, you’re probably familiar with the basic rules:

-   If a word ends in S, X, or Z, add ES. *Bass* becomes *basses*, *fax*
    becomes *faxes*, and *waltz* becomes *waltzes*.
-   If a word ends in a noisy H, add ES; if it ends in a silent H, just
    add S. What’s a noisy H? One that gets combined with other letters
    to make a sound that you can hear. So *coach* becomes *coaches* and
    *rash* becomes *rashes*, because you can hear the CH and SH sounds
    when you say them. But *cheetah* becomes *cheetahs*, because the H
    is silent.
-   If a word ends in Y that sounds like I, change the Y to IES; if the
    Y is combined with a vowel to sound like something else, just add S.
    So *vacancy* becomes *vacancies*, but *day* becomes *days*.
-   If all else fails, just add S and hope for the best.

(I know, there are a lot of exceptions. *Man* becomes *men* and *woman*
becomes *women*, but *human* becomes *humans*. *Mouse* becomes *mice*
and *louse* becomes *lice*, but *house* becomes *houses*. *Knife*
becomes *knives* and *wife* becomes *wives*, but *lowlife* becomes
*lowlifes*. And don’t even get me started on words that are their own
plural, like *sheep*, *deer*, and *haiku*.)

Other languages, of course, are completely different.

Let’s design a Python library that automatically pluralizes English
nouns. We’ll start with just these four rules, but keep in mind that
you’ll inevitably need to add more.

⁂

I Know, Let’s Use Regular Expressions! {#i-know}
--------------------------------------

So you’re looking at words, which, at least in English, means you’re
looking at strings of characters. You have rules that say you need to
find different combinations of characters, then do different things to
them. This sounds like a job for regular expressions!

[[download `plural1.py`](examples/plural1.py)]

~~~~ {.pp}
import re

def plural(noun):          
    if re.search('[sxz]$', noun):             ①
        return re.sub('$', 'es', noun)        ②
    elif re.search('[^aeioudgkprt]h$', noun):
        return re.sub('$', 'es', noun)       
    elif re.search('[^aeiou]y$', noun):      
        return re.sub('y$', 'ies', noun)     
    else:
        return noun + 's'
~~~~

1.  This is a regular expression, but it uses a syntax you didn’t see in
    [*Regular Expressions*](regular-expressions.html). The square
    brackets mean “match exactly one of these characters.” So `[sxz]`
    means “`s`, or `x`, or `z`”, but only one of them. The `$` should be
    familiar; it matches the end of string. Combined, this regular
    expression tests whether noun ends with `s`, `x`, or `z`.
2.  This `re.sub()` function performs regular expression-based string
    substitutions.

Let’s look at regular expression substitutions in more detail.

~~~~ {.screen}
>>> import re
>>> re.search('[abc]', 'Mark')    ①
<_sre.SRE_Match object at 0x001C1FA8>
>>> re.sub('[abc]', 'o', 'Mark')  ②
'Mork'
>>> re.sub('[abc]', 'o', 'rock')  ③
'rook'
>>> re.sub('[abc]', 'o', 'caps')  ④
'oops'
~~~~

1.  Does the string `Mark` contain `a`, `b`, or `c`? Yes, it contains
    `a`.
2.  OK, now find `a`, `b`, or `c`, and replace it with `o`. `Mark`
    becomes `Mork`.
3.  The same function turns `rock` into `rook`.
4.  You might think this would turn `caps` into `oaps`, but it doesn’t.
    `re.sub` replaces *all* of the matches, not just the first one. So
    this regular expression turns `caps` into `oops`, because both the
    `c` and the `a` get turned into `o`.

And now, back to the `plural()` function…

~~~~ {.pp}
def plural(noun):          
    if re.search('[sxz]$', noun):            
        return re.sub('$', 'es', noun)         ①
    elif re.search('[^aeioudgkprt]h$', noun):  ②
        return re.sub('$', 'es', noun)
    elif re.search('[^aeiou]y$', noun):        ③
        return re.sub('y$', 'ies', noun)     
    else:
        return noun + 's'
~~~~

1.  Here, you’re replacing the end of the string (matched by `$`) with
    the string `es`. In other words, adding `es` to the string. You
    could accomplish the same thing with string concatenation, for
    example `noun + 'es'`, but I chose to use regular expressions for
    each rule, for reasons that will become clear later in the chapter.
2.  Look closely, this is another new variation. The `^` as the first
    character inside the square brackets means something special:
    negation. `[^abc]` means “any single character *except* `a`, `b`, or
    `c`”. So `[^aeioudgkprt]` means any character except `a`, `e`, `i`,
    `o`, `u`, `d`, `g`, `k`, `p`, `r`, or `t`. Then that character needs
    to be followed by `h`, followed by end of string. You’re looking for
    words that end in H where the H can be heard.
3.  Same pattern here: match words that end in Y, where the character
    before the Y is *not* `a`, `e`, `i`, `o`, or `u`. You’re looking for
    words that end in Y that sounds like I.

Let’s look at negation regular expressions in more detail.

~~~~ {.screen}
>>> import re
>>> re.search('[^aeiou]y$', 'vacancy')  ①
<_sre.SRE_Match object at 0x001C1FA8>
>>> re.search('[^aeiou]y$', 'boy')      ②
>>> 
>>> re.search('[^aeiou]y$', 'day')
>>> 
>>> re.search('[^aeiou]y$', 'pita')     ③
>>> 
~~~~

1.  `vacancy` matches this regular expression, because it ends in `cy`,
    and `c` is not `a`, `e`, `i`, `o`, or `u`.
2.  `boy` does not match, because it ends in `oy`, and you specifically
    said that the character before the `y` could not be `o`. `day` does
    not match, because it ends in `ay`.
3.  `pita` does not match, because it does not end in `y`.

~~~~ {.screen}
>>> re.sub('y$', 'ies', 'vacancy')               ①
'vacancies'
>>> re.sub('y$', 'ies', 'agency')
'agencies'
>>> re.sub('([^aeiou])y$', r'\1ies', 'vacancy')  ②
'vacancies'
~~~~

1.  This regular expression turns `vacancy` into `vacancies` and
    `agency` into `agencies`, which is what you wanted. Note that it
    would also turn `boy` into `boies`, but that will never happen in
    the function because you did that `re.search` first to find out
    whether you should do this `re.sub`.
2.  Just in passing, I want to point out that it is possible to combine
    these two regular expressions (one to find out if the rule applies,
    and another to actually apply it) into a single regular expression.
    Here’s what that would look like. Most of it should look familiar:
    you’re using a remembered group, which you learned in [Case study:
    Parsing Phone Numbers](regular-expressions.html#phonenumbers). The
    group is used to remember the character before the letter `y`. Then
    in the substitution string, you use a new syntax, `\1`, which means
    “hey, that first group you remembered? put it right here.” In this
    case, you remember the `c` before the `y`; when you do the
    substitution, you substitute `c` in place of `c`, and `ies` in place
    of `y`. (If you have more than one remembered group, you can use
    `\2` and `\3` and so on.)

Regular expression substitutions are extremely powerful, and the `\1`
syntax makes them even more powerful. But combining the entire operation
into one regular expression is also much harder to read, and it doesn’t
directly map to the way you first described the pluralizing rules. You
originally laid out rules like “if the word ends in S, X, or Z, then add
ES”. If you look at this function, you have two lines of code that say
“if the word ends in S, X, or Z, then add ES”. It doesn’t get much more
direct than that.

⁂

A List Of Functions
-------------------

Now you’re going to add a level of abstraction. You started by defining
a list of rules: if this, do that, otherwise go to the next rule. Let’s
temporarily complicate part of the program so you can simplify another
part.

[[download `plural2.py`](examples/plural2.py)]

~~~~ {.pp}
import re

def match_sxz(noun):
    return re.search('[sxz]$', noun)

def apply_sxz(noun):
    return re.sub('$', 'es', noun)

def match_h(noun):
    return re.search('[^aeioudgkprt]h$', noun)

def apply_h(noun):
    return re.sub('$', 'es', noun)

def match_y(noun):                             ①
    return re.search('[^aeiou]y$', noun)
        
def apply_y(noun):                             ②
    return re.sub('y$', 'ies', noun)

def match_default(noun):
    return True

def apply_default(noun):
    return noun + 's'

rules = ((match_sxz, apply_sxz),               ③
         (match_h, apply_h),
         (match_y, apply_y),
         (match_default, apply_default)
         )

def plural(noun):           
    for matches_rule, apply_rule in rules:       ④
        if matches_rule(noun):
            return apply_rule(noun)
~~~~

1.  Now, each match rule is its own function which returns the results
    of calling the `re.search()` function.
2.  Each apply rule is also its own function which calls the `re.sub()`
    function to apply the appropriate pluralization rule.
3.  Instead of having one function (`plural()`) with multiple rules, you
    have the `rules` data structure, which is a sequence of pairs of
    functions.
4.  Since the rules have been broken out into a separate data structure,
    the new `plural()` function can be reduced to a few lines of code.
    Using a `for` loop, you can pull out the match and apply rules two
    at a time (one match, one apply) from the rules structure. On the
    first iteration of the `for` loop, matches\_rule will get
    `match_sxz`, and apply\_rule will get `apply_sxz`. On the second
    iteration (assuming you get that far), matches\_rule will be
    assigned `match_h`, and apply\_rule will be assigned `apply_h`. The
    function is guaranteed to return something eventually, because the
    final match rule (`match_default`) simply returns `True`, meaning
    the corresponding apply rule (`apply_default`) will always be
    applied.

The “rules” variable is a sequence of pairs of functions.

The reason this technique works is that [everything in Python is an
object](your-first-python-program.html#everythingisanobject), including
functions. The rules data structure contains functions — not names of
functions, but actual function objects. When they get assigned in the
`for` loop, then matches\_rule and apply\_rule are actual functions that
you can call. On the first iteration of the `for` loop, this is
equivalent to calling `matches_sxz(noun)`, and if it returns a match,
calling `apply_sxz(noun)`.

If this additional level of abstraction is confusing, try unrolling the
function to see the equivalence. The entire `for` loop is equivalent to
the following:

~~~~ {.nd .pp}
def plural(noun):
    if match_sxz(noun):
        return apply_sxz(noun)
    if match_h(noun):
        return apply_h(noun)
    if match_y(noun):
        return apply_y(noun)
    if match_default(noun):
        return apply_default(noun)
~~~~

The benefit here is that the `plural()` function is now simplified. It
takes a sequence of rules, defined elsewhere, and iterates through them
in a generic fashion.

1.  Get a match rule
2.  Does it match? Then call the apply rule and return the result.
3.  No match? Go to step 1.

The rules could be defined anywhere, in any way. The `plural()` function
doesn’t care.

Now, was adding this level of abstraction worth it? Well, not yet. Let’s
consider what it would take to add a new rule to the function. In the
first example, it would require adding an `if` statement to the
`plural()` function. In this second example, it would require adding two
functions, `match_foo()` and `apply_foo()`, and then updating the rules
sequence to specify where in the order the new match and apply functions
should be called relative to the other rules.

But this is really just a stepping stone to the next section. Let’s move
on…

⁂

A List Of Patterns
------------------

Defining separate named functions for each match and apply rule isn’t
really necessary. You never call them directly; you add them to the
rules sequence and call them through there. Furthermore, each function
follows one of two patterns. All the match functions call `re.search()`,
and all the apply functions call `re.sub()`. Let’s factor out the
patterns so that defining new rules can be easier.

[[download `plural3.py`](examples/plural3.py)]

~~~~ {.pp}
import re

def build_match_and_apply_functions(pattern, search, replace):
    def matches_rule(word):                                     ①
        return re.search(pattern, word)
    def apply_rule(word):                                       ②
        return re.sub(search, replace, word)
    return (matches_rule, apply_rule)                           ③
~~~~

1.  `build_match_and_apply_functions()` is a function that builds other
    functions dynamically. It takes pattern, search and replace, then
    defines a `matches_rule()` function which calls `re.search()` with
    the pattern that was passed to the
    `build_match_and_apply_functions()` function, and the word that was
    passed to the `matches_rule()` function you’re building. Whoa.
2.  Building the apply function works the same way. The apply function
    is a function that takes one parameter, and calls `re.sub()` with
    the search and replace parameters that were passed to the
    `build_match_and_apply_functions()` function, and the word that was
    passed to the `apply_rule()` function you’re building. This
    technique of using the values of outside parameters within a dynamic
    function is called *closures*. You’re essentially defining constants
    within the apply function you’re building: it takes one parameter
    (word), but it then acts on that plus two other values (search and
    replace) which were set when you defined the apply function.
3.  Finally, the `build_match_and_apply_functions()` function returns a
    tuple of two values: the two functions you just created. The
    constants you defined within those functions (pattern within the
    `matches_rule()` function, and search and replace within the
    `apply_rule()` function) stay with those functions, even after you
    return from `build_match_and_apply_functions()`. That’s insanely
    cool.

If this is incredibly confusing (and it should be, this is weird stuff),
it may become clearer when you see how to use it.

~~~~ {.pp}
patterns = \                                                        ①
  (
    ('[sxz]$',           '$',  'es'),
    ('[^aeioudgkprt]h$', '$',  'es'),
    ('(qu|[^aeiou])y$',  'y$', 'ies'),
    ('$',                '$',  's')                                 ②
  )
rules = [build_match_and_apply_functions(pattern, search, replace)  ③
         for (pattern, search, replace) in patterns]
~~~~

1.  Our pluralization “rules” are now defined as a tuple of tuples of
    *strings* (not functions). The first string in each group is the
    regular expression pattern that you would use in `re.search()` to
    see if this rule matches. The second and third strings in each group
    are the search and replace expressions you would use in `re.sub()`
    to actually apply the rule to turn a noun into its plural.
2.  There’s a slight change here, in the fallback rule. In the previous
    example, the `match_default()` function simply returned `True`,
    meaning that if none of the more specific rules matched, the code
    would simply add an `s` to the end of the given word. This example
    does something functionally equivalent. The final regular expression
    asks whether the word has an end (`$` matches the end of a string).
    Of course, every string has an end, even an empty string, so this
    expression always matches. Thus, it serves the same purpose as the
    `match_default()` function that always returned `True`: it ensures
    that if no more specific rule matches, the code adds an `s` to the
    end of the given word.
3.  This line is magic. It takes the sequence of strings in patterns and
    turns them into a sequence of functions. How? By “mapping” the
    strings to the `build_match_and_apply_functions()` function. That
    is, it takes each triplet of strings and calls the
    `build_match_and_apply_functions()` function with those three
    strings as arguments. The `build_match_and_apply_functions()`
    function returns a tuple of two functions. This means that rules
    ends up being functionally equivalent to the previous example: a
    list of tuples, where each tuple is a pair of functions. The first
    function is the match function that calls `re.search()`, and the
    second function is the apply function that calls `re.sub()`.

Rounding out this version of the script is the main entry point, the
`plural()` function.

~~~~ {.pp}
def plural(noun):
    for matches_rule, apply_rule in rules:  ①
        if matches_rule(noun):
            return apply_rule(noun)
~~~~

1.  Since the rules list is the same as the previous example (really, it
    is), it should come as no surprise that the `plural()` function
    hasn’t changed at all. It’s completely generic; it takes a list of
    rule functions and calls them in order. It doesn’t care how the
    rules are defined. In the previous example, they were defined as
    separate named functions. Now they are built dynamically by mapping
    the output of the `build_match_and_apply_functions()` function onto
    a list of raw strings. It doesn’t matter; the `plural()` function
    still works the same way.

⁂

A File Of Patterns
------------------

You’ve factored out all the duplicate code and added enough abstractions
so that the pluralization rules are defined in a list of strings. The
next logical step is to take these strings and put them in a separate
file, where they can be maintained separately from the code that uses
them.

First, let’s create a text file that contains the rules you want. No
fancy data structures, just whitespace-delimited strings in three
columns. Let’s call it `plural4-rules.txt`.

[[download `plural4-rules.txt`](examples/plural4-rules.txt)]

~~~~ {.nd .pp}
[sxz]$               $    es
[^aeioudgkprt]h$     $    es
[^aeiou]y$          y$    ies
$                    $    s
~~~~

Now let’s see how you can use this rules file.

[[download `plural4.py`](examples/plural4.py)]

~~~~ {.pp}
import re

def build_match_and_apply_functions(pattern, search, replace):  ①
    def matches_rule(word):
        return re.search(pattern, word)
    def apply_rule(word):
        return re.sub(search, replace, word)
    return (matches_rule, apply_rule)

rules = []
with open('plural4-rules.txt', encoding='utf-8') as pattern_file:  ②
    for line in pattern_file:                                      ③
        pattern, search, replace = line.split(None, 3)             ④
        rules.append(build_match_and_apply_functions(              ⑤
                pattern, search, replace))
~~~~

1.  The `build_match_and_apply_functions()` function has not changed.
    You’re still using closures to build two functions dynamically that
    use variables defined in the outer function.
2.  The global `open()` function opens a file and returns a file object.
    In this case, the file we’re opening contains the pattern strings
    for pluralizing nouns. The `with` statement creates what’s called a
    *context*: when the `with` block ends, Python will automatically
    close the file, even if an exception is raised inside the `with`
    block. You’ll learn more about `with` blocks and file objects in the
    [Files](files.html) chapter.
3.  The `for line in <fileobject>` idiom reads data from the open file,
    one line at a time, and assigns the text to the line variable.
    You’ll learn more about reading from files in the
    [Files](files.html) chapter.
4.  Each line in the file really has three values, but they’re separated
    by whitespace (tabs or spaces, it makes no difference). To split it
    out, use the `split()` string method. The first argument to the
    `split()` method is `None`, which means “split on any whitespace
    (tabs or spaces, it makes no difference).” The second argument is
    `3`, which means “split on whitespace 3 times, then leave the rest
    of the line alone.” A line like `[sxz]$ $ es` will be broken up into
    the list `['[sxz]$', '$', 'es']`, which means that pattern will get
    `'[sxz]$'`, search will get `'$'`, and replace will get `'es'`.
    That’s a lot of power in one little line of code.
5.  Finally, you pass `pattern`, `search`, and `replace` to the
    `build_match_and_apply_functions()` function, which returns a tuple
    of functions. You append this tuple to the rules list, and rules
    ends up storing the list of match and apply functions that the
    `plural()` function expects.

The improvement here is that you’ve completely separated the
pluralization rules into an external file, so it can be maintained
separately from the code that uses it. Code is code, data is data, and
life is good.

⁂

Generators
----------

Wouldn’t it be grand to have a generic `plural()` function that parses
the rules file? Get rules, check for a match, apply appropriate
transformation, go to next rule. That’s all the `plural()` function has
to do, and that’s all the `plural()` function should do.

[[download `plural5.py`](examples/plural5.py)]

~~~~ {.nd .pp}
def rules(rules_filename):
    with open(rules_filename, encoding='utf-8') as pattern_file:
        for line in pattern_file:
            pattern, search, replace = line.split(None, 3)
            yield build_match_and_apply_functions(pattern, search, replace)

def plural(noun, rules_filename='plural5-rules.txt'):
    for matches_rule, apply_rule in rules(rules_filename):
        if matches_rule(noun):
            return apply_rule(noun)
    raise ValueError('no matching rule for {0}'.format(noun))
~~~~

How the heck does *that* work? Let’s look at an interactive example
first.

~~~~ {.screen}
>>> def make_counter(x):
...     print('entering make_counter')
...     while True:
...         yield x                    ①
...         print('incrementing x')
...         x = x + 1
... 
>>> counter = make_counter(2)          ②
>>> counter                            ③
<generator object at 0x001C9C10>
>>> next(counter)                      ④
entering make_counter
2
>>> next(counter)                      ⑤
incrementing x
3
>>> next(counter)                      ⑥
incrementing x
4
~~~~

1.  The presence of the `yield` keyword in `make_counter` means that
    this is not a normal function. It is a special kind of function
    which generates values one at a time. You can think of it as a
    resumable function. Calling it will return a *generator* that can be
    used to generate successive values of x.
2.  To create an instance of the `make_counter` generator, just call it
    like any other function. Note that this does not actually execute
    the function code. You can tell this because the first line of the
    `make_counter()` function calls `print()`, but nothing has been
    printed yet.
3.  The `make_counter()` function returns a generator object.
4.  The `next()` function takes a generator object and returns its next
    value. The first time you call `next()` with the counter generator,
    it executes the code in `make_counter()` up to the first `yield`
    statement, then returns the value that was yielded. In this case,
    that will be `2`, because you originally created the generator by
    calling `make_counter(2)`.
5.  Repeatedly calling `next()` with the same generator object resumes
    exactly where it left off and continues until it hits the next
    `yield` statement. All variables, local state, *&*c. are saved on
    `yield` and restored on `next()`. The next line of code waiting to
    be executed calls `print()`, which prints incrementing x. After
    that, the statement `x = x + 1`. Then it loops through the `while`
    loop again, and the first thing it hits is the statement `yield x`,
    which saves the state of everything and returns the current value of
    x (now `3`).
6.  The second time you call `next(counter)`, you do all the same things
    again, but this time x is now `4`.

Since `make_counter` sets up an infinite loop, you could theoretically
do this forever, and it would just keep incrementing x and spitting out
values. But let’s look at more productive uses of generators instead.

### A Fibonacci Generator

“yield” pauses a function. “next()” resumes where it left off.

[[download `fibonacci.py`](examples/fibonacci.py)]

~~~~ {.pp}
def fib(max):
    a, b = 0, 1          ①
    while a < max:
        yield a          ②
        a, b = b, a + b  ③
~~~~

1.  The Fibonacci sequence is a sequence of numbers where each number is
    the sum of the two numbers before it. It starts with 0 and `1`, goes
    up slowly at first, then more and more rapidly. To start the
    sequence, you need two variables: a starts at 0, and b starts at
    `1`.
2.  a is the current number in the sequence, so yield it.
3.  b is the next number in the sequence, so assign that to a, but also
    calculate the next value (`a + b`) and assign that to b for later
    use. Note that this happens in parallel; if a is `3` and b is `5`,
    then `a, b = b, a + b` will set a to `5` (the previous value of b)
    and b to `8` (the sum of the previous values of a and b).

So you have a function that spits out successive Fibonacci numbers.
Sure, you could do that with recursion, but this way is easier to read.
Also, it works well with `for` loops.

~~~~ {.screen}
>>> from fibonacci import fib
>>> for n in fib(1000):      ①
...     print(n, end=' ')    ②
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> list(fib(1000))          ③
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987]
~~~~

1.  You can use a generator like `fib()` in a `for` loop directly. The
    `for` loop will automatically call the `next()` function to get
    values from the `fib()` generator and assign them to the `for` loop
    index variable (n).
2.  Each time through the `for` loop, n gets a new value from the
    `yield` statement in `fib()`, and all you have to do is print it
    out. Once `fib()` runs out of numbers (a becomes bigger than max,
    which in this case is `1000`), then the `for` loop exits gracefully.
3.  This is a useful idiom: pass a generator to the `list()` function,
    and it will iterate through the entire generator (just like the
    `for` loop in the previous example) and return a list of all the
    values.

### A Plural Rule Generator

Let’s go back to `plural5.py` and see how this version of the `plural()`
function works.

~~~~ {.pp}
def rules(rules_filename):
    with open(rules_filename, encoding='utf-8') as pattern_file:
        for line in pattern_file:
            pattern, search, replace = line.split(None, 3)                   ①
            yield build_match_and_apply_functions(pattern, search, replace)  ②

def plural(noun, rules_filename='plural5-rules.txt'):
    for matches_rule, apply_rule in rules(rules_filename):                   ③
        if matches_rule(noun):
            return apply_rule(noun)
    raise ValueError('no matching rule for {0}'.format(noun))
~~~~

1.  No magic here. Remember that the lines of the rules file have three
    values separated by whitespace, so you use `line.split(None, 3)` to
    get the three “columns” and assign them to three local variables.
2.  *And then you yield.* What do you yield? Two functions, built
    dynamically with your old friend,
    `build_match_and_apply_functions()`, which is identical to the
    previous examples. In other words, `rules()` is a generator that
    spits out match and apply functions *on demand*.
3.  Since `rules()` is a generator, you can use it directly in a `for`
    loop. The first time through the `for` loop, you will call the
    `rules()` function, which will open the pattern file, read the first
    line, dynamically build a match function and an apply function from
    the patterns on that line, and yield the dynamically built
    functions. The second time through the `for` loop, you will pick up
    exactly where you left off in `rules()` (which was in the middle of
    the `for line in pattern_file` loop). The first thing it will do is
    read the next line of the file (which is still open), dynamically
    build another match and apply function based on the patterns on that
    line in the file, and yield the two functions.

What have you gained over stage 4? Startup time. In stage 4, when you
imported the `plural4` module, it read the entire patterns file and
built a list of all the possible rules, before you could even think
about calling the `plural()` function. With generators, you can do
everything lazily: you read the first rule and create functions and try
them, and if that works you don’t ever read the rest of the file or
create any other functions.

What have you lost? Performance! Every time you call the `plural()`
function, the `rules()` generator starts over from the beginning — which
means re-opening the patterns file and reading from the beginning, one
line at a time.

What if you could have the best of both worlds: minimal startup cost
(don’t execute any code on `import`), *and* maximum performance (don’t
build the same functions over and over again). Oh, and you still want to
keep the rules in a separate file (because code is code and data is
data), just as long as you never have to read the same line twice.

To do that, you’ll need to build your own iterator. But before you do
*that*, you need to learn about Python classes.

⁂

Further Reading {#furtherreading}
---------------

-   [PEP 255: Simple
    Generators](http://www.python.org/dev/peps/pep-0255/)
-   [Understanding Python’s “with”
    statement](http://effbot.org/zone/python-with-statement.htm)
-   [Closures in
    Python](http://ynniv.com/blog/2007/08/closures-in-python.html)
-   [Fibonacci numbers](http://en.wikipedia.org/wiki/Fibonacci_number)
-   [English Irregular Plural
    Nouns](http://www2.gsu.edu/~wwwesl/egw/crump.htm)

[☜](regular-expressions.html "back to “Regular Expressions”")
[☞](iterators.html "onward to “Classes & Iterators”")

© 2001–11 [Mark Pilgrim](about.html)
