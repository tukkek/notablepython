# Notable Python features

Python's immense success goes hand-in-hand with the fact that it is one of easist-to-read, easiest-to-write programming languages ever invented, while offering as many features and syntatic sugar as even most major languages out there.  Python programmers are well-acquainted, for example, with powerful syntax such as list slicing (`mylist[1:-1]`) and many handy operators (from `[1,2,3] + [4,5,6]` to `'hue'*3`). This project's goal is to try and document some of the features that are less widely used, but just as powerful and idiomatic that one can use in their own code.

Keep in mind, however, that there is a reason why some of these are not found out in the wild very frequently: programmers, especially those who work in a variety of languages, usually prefer to stay with standard methods of doing things rather than using a novelty mechanism just for the sake of it. While these may make your code a few lines shorter, it does so at the expense of readability, as even other Python programmers may not be familiar enough to immediately understand what's going on. Python code is already very short compared to most standard programming languages so really, most of the time, there isn't any need to sacrifice readibility just to reduce your file by a couple of lines of code.

On the other hand, sometimes these can greatly simplify the solution to a problem or speed up its execution - or maybe you're just writing code for yourself and you don't care about readability issues since no one else will be looking at it. Anyway, Python is very meticulous about introducing syntax into the language itself so even if it's new, most of the time it shouldn't be too esoteric for anyone to figure out what is happening... especially if you include a comment with a link to this document, perhaps ;)

Feel free to submit [a new issue to this project's tracker](https://github.com/tukkek/notablepython/issues/new) or a pull request if you feel like I have missed out on something. However, keep in mind that I have to draw the line between "underutilized" and "standard" Python somewhere, which is why you won't see things such as decorators or generators being discussed here, even though a lot of begginer Python coders would find them to be somewhat esoteric upon first encountering them, especially if they're coming from older languages or just now learning how to code.

This document is up-to-date with **Python 3.7.0**.

- [Notable Python features](#notable-python-features)
  * [Boolean arithmetic](#boolean-arithmetic)
  * [Built-in HTTP server](#built-in-http-server)
  * [Chaining comparison operators](#chaining-comparison-operators)
  * [Concatenating strings](#concatenating-strings)
  * [Console formatting for strings](#console-formatting-for-strings)
  * [Context manager decorations](#context-manager-decorations)
  * [Dictionary comprehensions](#dictionary-comprehensions)
  * [Formatting strings](#formatting-strings)
  * [Generator expressions](#generator-expressions)
  * [In-place value swapping](#in-place-value-swapping)
  * [List slice assignment](#list-slice-assignment)
  * [Multi-line strings](#multi-line-strings)
  * [Structured data](#structured-data)
  * [Unconventional else blocks](#unconventional-else-blocks)
  * [Zen of Python](#zen-of-python)
- [Honorable mentions](#honorable-mentions)
- [Acknowledgments](#acknowledgments)

## Boolean arithmetic

```py
css_class='button_selected' if selected else ''
css_class='button_selected'*selected
html_button='<button type="button" class="{}">Click Me!</button>'.format(css_class)
```

In the example above both ways of defining the `css_class` string are equivalent. The second method is useful in a number of scenarios and works because in Python booleans are a subclass of integers (`assert isinstance(True,int)`), with `True` resolving to 1 and `False` resolving to 0. 

Source: https://stackoverflow.com/a/1853593

```py
text = 'Some example data'
vowels = 'aeiou'
vowel_count = sum(char in vowels for char in text)
print(vowel_count) #prints: 7
```

This other snippet works because the generator function used with `sum()` supplies a boolean result for each execution of `char in vowels` (once per character in the given text). `sum()` then goes to treat each of those as either a 0 or a 1, finding the total count.

Source: https://www.reddit.com/r/Python/comments/6xc33n/notable_python_collection_of_lesser_known_python/dmg24zj/

## Built-in HTTP server

From a terminal you can execute this command to start a HTTP server which serves the content of the current working directory (usually the one you are in when executing the command itself): `python3 -m http.server`. The equivalent command for Python 2 is `python2 -m SimpleHTTPServer 8000`. You can then access it by directing your browser (or any other program) to `localhost:8000`.

Not anywhere near feature-complete but extremely useful in certain situations or for very simple tasks, if only due to the fact that it's built-in into Python itself, making it very easy to boot. You can also access the server programatically if you need more control - just read its [module documentation](https://docs.python.org/3.7/library/http.server.html?highlight=http%20server#module-http.server). Needless to say, this server is not to be used in production or for public-facing purposes.

Source: http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html

## Chaining comparison operators

```py
x = 5
1 < x < 10 #True
10 < x < 20 #False
x < 10 < x*10 < 100 #True
```

The way this works is that `a == b == c` is equivalent to `(a == b) and (b == c)`. Note, however that this can create some counter-intuitive cases if used carelessly, like the one below:

```py
True is False == False #False
```

Sources: [Official Python documentation](https://docs.python.org/3/tutorial/datastructures.html#more-on-conditions), [Stack Overflow](https://stackoverflow.com/a/101945).

## Concatenating strings

```py
chunks = []
for s in my_strings:
    chunks.append(s)
result = ''.join(chunks)
```

The example above shows the official recommended way of concatenating a large number of strings together when performance is a concern. 

Source: https://docs.python.org/3/faq/programming.html#what-is-the-most-efficient-way-to-concatenate-many-strings-together

## Console formatting for strings

```py
hello='hello world'
print(hello.ljust(20,'-'))
print(hello.center(20,'.'))
print(hello.rjust(20))

'''
Prints:

hello world---------
....hello world.....
         hello world
'''
```

These 3 methods are useful for any type of display that uses fixed-width fonts. They are also available with [bytes and bytearrays](https://docs.python.org/3/library/stdtypes.html#bytes-and-bytearray-operations). If you need to further wrap the text, there is [a module](https://docs.python.org/3/library/textwrap.html) just for that.

User @artieleach points out that Python 3 also offers a more powerful string formatting syntax, introduced by [PEP 3101](https://www.python.org/dev/peps/pep-3101/) - with `print('{:>20}'.format(hello))` achieving the same as `rjust()` above.

Python also exposes [an interface to the curses library](https://docs.python.org/3/library/curses.html), which is the de-facto way to create complex, responsive terminal-based user interfaces.

## Context manager decorations

```py
from contextlib import contextmanager

@contextmanager
def context_log(module):
    print('['+module+'] before...')
    yield module
    print('['+module+'] after...')

with context_log('Example 1') as module:
    print('Executing module '+module+'...')
    
# Prints:
# [Example 1] before...
# Executing module Example 1...
# [Example 1] after...
```

Did you know you could declare a [context manager](https://docs.python.org/3/glossary.html#term-context-manager) simply by adding a decoration to a function? This is definitely much less verbose than creating an actual class with the relevant special methods!


```py
@context_log('Example 2')
def execute_module():
    print('Executing another module...')

execute_module()

# Prints:
# [Example 2] before...
# Executing another module...
# [Example 2] after...
```

Now for some real Python-ception! You can use your brand new context manager (which was created using a decorator and a function) as a decorator for another function... Is your mind blown yet? 

In this second case, though, you won't have access to the context variable returned by your manager.since the decoration itself lies outside the function scope.

Source: https://docs.python.org/3/whatsnew/3.2.html#contextlib

## Dictionary comprehensions

```py
alice=dict(name='Alice',email='alice@python.org',age=20)
bob=dict(name='Bob',email='bob@python.org',age=30)
eve=dict(name='Eve',email='eve@python.org',age=40)
people=[alice,bob,eve]
people_by_email={person['email']: person for person in people}
print(people_by_email.keys()) 

# prints: dict_keys(['alice@python.org', 'bob@python.org', 'eve@python.org'])
```

List comprehensions are quite common but not all Python coders know that you can use pretty much the same syntax to work with dictionaries instead! 

## Formatting strings

```py
name = 'Fred'
language = 'Python'
print(f'{name} likes to write code in {language}.')

# prints: Fred likes to write code in Python.
```

One of the few things that have changed a lot in the language is how to format strings. Besides constructinrg your own strings manually, in Python 2 you had to use the `%` operator if you wanted structured output, which was then replaced in 2.6 with the more pythonic [string.format()](https://docs.python.org/3/library/stdtypes.html#str.format) method (which in turn is a delegate for [`object.__format__()`](https://docs.python.org/3.6/reference/datamodel.html#object.__format__)). Both of these methods, although very similar to the classic C `printf()` function, could easily lead to long, messy lines - especially when working with a complex formatter or one that contains more than a few variables. 

Thankfully, as of Python 3.6, a new type of built-in string called a Formatted String Literal (declared by using `f''`) goes to solve this issue. The string will automatically resolve to its value, as per [the usual formatting syntax](https://docs.python.org/3/library/string.html#formatstrings) - except it takes no method calls or arguments, simply executing the given expressions using the current local context. This goes a long way to improve readibility - resulting in a much cleaner vertical style (more and shorter lines) than the previous syntax, in which it was easy to produce hard-to-read horizontal code (one very long line).

```py
numbers=[5,1,9,40,356,18]
formatted=f'''
Highest value: {max(numbers)}
Number of elements: {len(numbers)}
Average value: {sum(numbers)/len(numbers)}
'''
print(formatted)

# Prints:
# Highest value in list: 356
# Number of elements in list: 6
# Average value: 71.5
```

As you can see, these formatted string literals aren't only a shorthand for formatting named values but in fact can contain any type of Python expression! This allows for a lot more uses than previous formatting mechanisms but can also produce harder-to-read code if abused.

Source: https://www.python.org/dev/peps/pep-0498

```py
alice=dict(name='Alice',email='alice@python.org',age=20)
print('{name} is {age} years old.'.format_map(alice))

# Prints: Alice is 20 years old.
```

If you want to format strings by using mapping data structures (such as dictionaries), you can go back to the traditional formatted strings and use their [format_map()](https://docs.python.org/3/library/stdtypes.html#str.format_map) function.

## Generator expressions

```py
sum_of_squares=sum(x*x for x in range(10))
words=set(word for line in page for word in line.split())
```

Generator expressions look a whole lot like list comprehensions but instead of using bracket syntax, they have parenthesis around them (which can be omitted a lot of the time, such as in the examples above). Their main advantage is that since they produce a [generator](https://wiki.python.org/moin/Generators) instead of a list, they don't have to instantiate and populate an object ahead of time, working instead as a lazy iterator function. Depending on what you are trying to do, it could have a major impact in your memory usage and overall performance - for that reason, whenever possible, you should use generator expressions instead of list comprehensions. Thankfully they are pretty easy to convert to and from! 

Source: https://www.python.org/dev/peps/pep-0289/

## In-place value swapping

```py
a,b = 1,2
a,b = b,a
print(a,b) # prints "2 1"
```

Simple trick for a fairly common programming scenario, removing the need for a placeholder variable.

Source: https://stackoverflow.com/a/102037

## List slice assignment

```py
a=list(range(1,9+1))
print(a) #prints: [1, 2, 3, 4, 5, 6, 7, 8, 9]
a[1:-1]=[-x for x in a[1:-1]]
print(a) #prints: [1, -2, -3, -4, -5, -6, -7, -8, 9]
a[1:]=['b','c','d']
print(a) #prints: [1, 'b', 'c', 'd']
a[-1:]=['d','e','f','g']
print(a) #prints: [1, 'b', 'c', 'd', 'e', 'f', 'g']
a[1:-1]=[]
print(a) #prints: [1, 'g']
```

Turns out list slicing syntax is not only useful for reading lists but also for writing to them! 

Source: http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html

## Multi-line strings

```py
sql = '''select * from some_table 
         where id > 10
         order by name'''

sql = ('select * from some_table '
       'where id > 10 '
       'order by name') 
```

Usually, when asked about formatting long strings, people will refer you to the triple-quote syntax (first example above). However, this has the substantial downside of taking all that whitespace (and newline characters) into your string. Sometimes this has no impact at all but on other times it makes your output nigh impossible to read later on - if you are trying to write HTML, for example. You shouldn't have to choose between beautiful, easy-to-read code at the source level or at the final output level, so using the parenthesis notation can be a real helper in such situations!

Source: https://stackoverflow.com/a/3342952

## Structured data

Python 3.7 introduces Data Classes as the superior method for easily handling data objects in Python. Prior to 3.7, programmers had various benefits and deficits with the several options for structured data (namedtuples, custom classes and using basic dictionaries). Data Classes work to a large extent to be a fully-featured replacement to all those legacy options, following one of Python's core guidelines of "there should only be one obvious way to do it".

To create a Data Class, all you need to do is add the `@dataclass` annotation to a barebones class declartion, which will then proceed to modify your class in-place:

```
@dataclass
class InventoryItem:
    '''Class for keeping track of an item in inventory.'''
    name: str
    unit_price: float
    quantity_on_hand: int = 0

    def total_cost(self) -> float:
        return self.unit_price * self.quantity_on_hand
```

The resulting class will be greatly expanded with methods for initialization, string representation, comparison, ordering (optional), hashing (automatic only if safe), getters, setters and immutability (optional). You can further determine how the generated class works by passing [parameters to the @dataclass annotation](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass) or using [field() objects](https://docs.python.org/3/library/dataclasses.html#dataclasses.field) to describe the behavior of your class fields.

Compared to the older data approaches to Python, Data Classes are type-friendly, allow for the full scope of object-oriented programming: inheritance (between plain classes and other Data Classes), static fields and methods, metaclasses... You can also easily work with legacy code that expects tuples or dictionaries by using the helper methods [dataclasses.asdict()](https://docs.python.org/3/library/dataclasses.html#dataclasses.asdict) and [dataclasses.astuple()](https://docs.python.org/3/library/dataclasses.html#dataclasses.astuple).

Although a powerful and versatile tool, Data Classes, as the name implies, are meant for the specific case of classes that mostly handle structured data. In some more complex cases, you may want to write a custom class yourself instead of relying on automatic generation provided by this approach. The third-party module [attrs]() is a similar tool to Data Classes but providing even more features (from validation to metadata) that might work better to more complex tasks.

## Unconventional "else" blocks


```py
def contains(number,mylist):
      for item in mylist:
          if item == number:
              print("Exists!")
              break
      else: # executed if "break" above isn't reached
          print("Does not exist") 
```

The `else` block will be executed after iteration is over, unless it is forcefully exited by the `break` statement. This works in the same way for the `while` statement.

```py
try: #run some code
    print(mymap['mykey'])
except KeyError as e: # executed only if the "try" block throws a KeyError exception
    print("KeyError thrown: "+str(e))
except: # executed only if "try" block throws any other exception type
    print("Other exception type thrown: "+str(e))
else: # executed only if "try" block does not throw any exception at all
    print("Try block executed successfully.")
finally: # always executed
    print('Done.') 
```

In the case of a `try-except` statement, the `else` block is only executed if no exceptions were thrown from inside the `try` block itself. Unlike the `finally` block, you need at least one `except` block if you're going to use `else` as well.

## Zen of Python

```py
import this
```

More of an easter egg than anything else - but a very wise one, at that :) It will output the contents of [PEP 20](https://www.python.org/dev/peps/pep-0020/), which in turn tells you what Python is all about! 

# Honorable mentions

This section covers, in brief, tools that can be very helpful for specific purposes.

* **[argparse](https://docs.python.org/3/library/argparse.html)** - framework for handling command-line arguments. [getopt](https://docs.python.org/3/library/getopt.html) allows for doing the same with a C-style approach. [docopt](http://docopt.org/) is a pythonic third-party library that offers a [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) for describing your command-line input and options.
* If the options in the *Structured Data* section aren't enough for you, the **[attrs](http://www.attrs.org/en/stable/examples.html)** third-party module allows for the creation of data classes, supporting validation, conversion, metadata and more!
* **[copy](https://docs.python.org/3/library/copy.html)** - object cloning module.
* **Comparison utilities** - [filecmp](https://docs.python.org/3/library/filecmp.html) for files and directories and [difflib](https://docs.python.org/3/library/difflib.html) for text.
* **[Concurrency](https://docs.python.org/3/library/concurrency.html)** - overview of the many parallelism approaches in Python.
* **[Counter dictionary](https://docs.python.org/3/library/collections.html#collections.Counter)** - counts the number of occurances of items in other collections.
* **[decimal](https://docs.python.org/3/library/decimal.html)** - precise [Numbers](https://docs.python.org/3/library/numbers.html).
* **[difflib.get_close_matches()](https://docs.python.org/3/library/difflib.html#difflib.get_close_matches)** - can be used to auto-correct user input (such as program arguments) on non-critical use cases.
* **[enum](https://docs.python.org/3/library/enum.html)** - Python version of enumerations. Allows creation through subclassing or via a function call. Supports pickle (see below).
* **File format support:**
    * **[configparser](https://docs.python.org/3/library/configparser.html)** - easily read from and write to user-facing configuration files (.ini).
    * **[HTML](https://docs.python.org/3/library/html.html)**,  **[JSON](https://docs.python.org/3/library/json.html)** and **[XML](https://docs.python.org/3/library/xml.html)**.
* **[Internationalization](https://docs.python.org/3/library/i18n.html)** - helper modules to implement standard i18n and l10n practices, such as adding support for text translations.
* **[pickle](https://docs.python.org/3/library/pickle.html)** - object serialization, complemented by [shelve](https://docs.python.org/3/library/shelve.html#module-shelve) for data persistence.
* **[Requests](http://docs.python-requests.org/en/master/)** - third-party library to replace most of the the standard [urllib](https://docs.python.org/3/library/urllib.html) module's HTTP functionality.
* The **[secrets](https://docs.python.org/3.6/library/secrets.html#module-secrets)** module allows for generation of tokens, password salt and similar in a secure manner. The official documentation states the `random` module should not be used for secure operations!
* **[statistics](https://docs.python.org/3/library/statistics.html)** - offers default implementations for things like `mean()` and `median()`.

# Acknowledgments

I'd like to thank Satwik Kansal for the [What the f\*ck, Python?](https://github.com/satwikkansal/wtfPython) snippet compilation. I have started this reference a couple of times in the past but seeing how neat and enjoyable his own documentation was, it renewed my focus to get this done and maybe be as fun and useful to read as I found his own project to be!

Table of contents generated with [markdown-toc](http://ecotrust-canada.github.io/markdown-toc/).
