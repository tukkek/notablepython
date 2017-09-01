# Notable Python features

Python's immense success goes hand-in-hand with the fact that it is one of easist-to-read, easiest-to-write programming languages ever invented, while offering as many features and syntatic sugar as even most major languages out there.  Python programmers are well-acquainted, for example, with powerful syntax such as list slicing (`mylist[1:-1]`) and many handy operators (from `[1,2,3] + [4,5,6]` to `'hue'*3`). This project's goal is to try and document some of the features that are less widely used, but just as powerful and idiomatic that one can use in their own code.

Keep in mind, however, that there is a reason why some of these are not found out in the wild very frequently: programmers, especially those who work in a variety of languages, usually prefer to stay with standard methods of doing things rather than using a novelty mechanism just for the sake of it. While these may make your code a few lines shorter, it does so at the expense of readilibity, as even other Python programmers may not be familiar enough to immediately understand what's going on. Python code is already very short compared to most standard programming languages so really, most of the time, there isn't any need to sacrifice readibility just to reduce your file by a couple of lines of code.

On the other hand, sometimes these can greatly simplify the solution to a problem or speed up its execution - or maybe you're just writing code for yourself and you don't care about readibility issues since no one else will be looking at it. Anyway, Python is very meticulous about introducing syntax into the language itself so even if it's new, most of the time it shouldn't be too esoteric for anyone to figure out what is happening... especially if you include a comment with a link to this document, perhaps ;)

Feel free to submit [a new issue to this project's tracker](https://github.com/tukkek/notablepython/issues/new) or a pull request if you feel like I have missed out on something. However, keep in mind that I have to draw the line between "underutilized" and "standard" Python somewhere, which is why you won't see things such as decorators or generators being discussed here, even though a lot of begginer Python coders would find them to be somewhat esoteric upon first encountering them, especially if they're coming from older languages or just now learning how to code.

This document is up-to-date with **Python 3.6.2**.

## Built-in HTTP server

From a terminal you can execute this command to start a HTTP server which serves the content of the current working directory (usually the one you are in when executing the command itself): `python3 -m http.server`. The equivalent command for Python 2 is `python2 -m SimpleHTTPServer 8000`. You can then access it by directing your browser (or any other program) to `localhost:8000`.

Not anywhere near feature-complete but extremely useful in certain situations or for very simple tasks, if only due to the fact that it's built-in into Python itself, making it very easy to boot. You can also access the server programatically if you need more control, just read the [package documentation](https://docs.python.org/3.7/library/http.server.html?highlight=http%20server#module-http.server). Needless to say, this server is not to be used in production or for public-facing purposes.

Source: http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html

## Chaining comparison operators

```py
x = 5
1 < x < 10 #True
10 < x < 20 #False
x < 10 < x*10 < 100 #True
```

The way this works is that `a == b == c` equivalent to `(a and b) and (b and c)`. Note, however that this can create some counter-intuitive cases if abused, like the one below:

```py
True is False == False #False
```

Source: https://stackoverflow.com/a/101945

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

These 3 methods are useful for any type of diplay that uses fixed-width fonts. They are also available with [bytes and bytearrays](https://docs.python.org/3/library/stdtypes.html#bytes-and-bytearray-operations). If you need to further wrap the text, Python offers [an entire module](https://docs.python.org/3/library/textwrap.html) to help with that.

## Dictionary comprehension

```py
class Person:
    def __init__(self,name,email,age):
        self.name=name
        self.email=email
        self.age=age
    def __repr__(self):
        return "User "+self.name
       
alice=Person('Alice','alice@python.org','20')
bob=Person('Bob','bob@python.org','30')
eve=Person('Eve','eve@python.org','40')
people=[alice,bob,eve]
people_by_email={person.email: person for person in people}
print(people_by_email)

# prints: {'bob@python.org': User Bob, 'eve@python.org': User Eve, 'alice@python.org': User Alice}
```

List comprehensions are quite common but not all Python coders know that you can use pretty much the same syntax to work with dictionaries! 

## Generator expressions

```py
sum_of_squares=sum(x*x for x in range(10))
words=Set(word for line in page for word in line.split())
```

Generator expressions look a whole lot like list comprehensions but instead of using bracket syntax, they have parenthesis around them (which can be omitted a lot of the time, such as in the examples above). Their main advantage is that since they produce a [generator](https://wiki.python.org/moin/Generators) instead of a list, they don't have to instantiate and populate an object ahead of time, working instead as a lazy iterator function. Depending on what you are trying to do, it could have a major impact in your memory usage and overall performance - for that reason, whenever possible, you should use generator expressions instead of list comprehensions. Thankfully they are pretty easy to convert to and from! Source: https://www.python.org/dev/peps/pep-0289/

## In-place value swapping

```py
a,b = 1,2
a,b = b,a
print(a,b) # prints "2 1"
```

Simple trick but removes the need for a placeholder variable, in a fairly common scenario. Source: https://stackoverflow.com/a/102037

## List slice assignment

```py
a=list(range(1,10))
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

Turns out list slicing syntax is not only useful for reading lists but also for writing to them! Source: http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html

## Multi-line strings

```py
sql = '''select * from some_table 
         where id > 10
         order by name'''

sql = ('select * from some_table '
       'where id > 10 '
       'order by name') 
```

Usually, when asked about formatting long strings, people will refer you to the triple-quote syntax (first example above). However, this has the substantial downside of taking all that whitespace (and newline characters) into your string. Sometimes this has no impact at all but on other times it makes your output nigh impossible to read later on - if you are trying to write HTML, for example. You shouldn't have to choose between beautiful, easy-to-read code in the source level or at the final output level, so using the parenthesis notation can be a real helper in such situations. Source: https://stackoverflow.com/a/3342952

## Multiplying strings by booleans

```py
css_class='button_selected' if selected else ''
css_class='button_selected'*selected
button='<button value="Click me" class="{}"/>'.format(css_class)
```

In the above example both methods of defining the `css_class` string are equivalent. This simple hack is useful in a number of scenarios and works because in Python booleans are a subclass of integer (`assert isinstance(True,int)`), with `True` resolving to 1 and `False` resolving to 0. Source: https://stackoverflow.com/a/1853593

## Unconventional `else` blocks


```py
def does_exists_num(list, to_find):
      for num in list:
          if num == to_find:
              print("Exists!")
              break
      else: # executed if "break" above isn't reached
          print("Does not exist") 
```
The `else` block will be executed after iteration is over, unless it is forcefully exited by a `break`. This works in the same way for the `while` statement

```py
try: #run some code
    print(mymap['mykey'])
except KeyError as e: # executed only if "try" block throws a KeyError exception
    print("KeyError thrown: "+str(e))
except: # executed only if "try" block throws any other exception type
    print("Other exception type thrown: "+str(e))
else: # executed only if "try" block does not throw any exception
    print("Try block executed successfully.")
finally: # always executed
    print('Done.') 
```

Note that every block besides "try" itself is optional - but you must have at least one of them to create a valid "try" clause - and you need at least one "except" block though to use "else". This means you can have only a try-finally or a simple try-except.

## Zen of Python

```py
import this
```

More of an easter egg but a very wise one, at that :) It will output the contents of [PEP 20](https://www.python.org/dev/peps/pep-0020/), which tells you what Python is all about! Source: https://stackoverflow.com/a/101276

# Honorable mentions

This section covers, in brief, lesser-known methods and libraries that, while less notable or not part of the standard Python distribution, can still be very helpful for one reason or another.

* **[copy](ttps://docs.python.org/3/library/copy.html)** - object cloning module.
* **[Comparison utilities]** - [filecmp](https://docs.python.org/3/library/filecmp.html) for files and directories and [difflib](https://docs.python.org/3/library/difflib.html) for text.
* **[Counter dictionary](https://docs.python.org/3/library/collections.html#collections.Counter)** - counts the number of occurances of items in other collections.
* **[decimal](https://docs.python.org/3/library/decimal.html)** - precise decimal [Numbers](https://docs.python.org/3/library/numbers.html).
* **[difflib.get_close_matches()](https://docs.python.org/3/library/difflib.html#difflib.get_close_matches)** - can auto-correct user input (such as program arguments) on non-critical use cases.
* **[enum](https://docs.python.org/3/library/enum.html)** - Python version of enumerations. Allows creation through subclassing or via a function call. Supports [pickle](https://docs.python.org/3/library/pickle.html) serialization.
* **[statistics](https://docs.python.org/3/library/statistics.html)** - offers default implementations of things like `mean()` and `median()`.


# Acknowledgments

I'd like to thank Satwik Kansal for the [What the f\*ck, Python?](https://github.com/satwikkansal/wtfPython) project. I have started this reference a couple of times in the past but seeing how neatly and enjoyable his own documentation was, it renewed my focus to get this done and hopefully be as fun/useful to read as his own was!
