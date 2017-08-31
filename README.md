# Notable Python features

Python's immense success goes hand-in-hand with the fact that it is one of easist-to-read, easiest-to-write programming languages ever invented, while offering as many features and syntatic sugar as even most major languages out there.  Python programmers are well-acquainted, for example, with powerful syntax such as list slicing (`mylist[1:-1]`) and many handy operators (from `[1,2,3] + [4,5,6]` to `'hue'*3`). This project's goal is to try and document some of the features that are less widely used, but just as powerful and idiomatic that one can use in their own code.

Keep in mind, however, that there is a reason why some of these are not found out in the wild very frequently: programmers, especially those who work in a variety of languages, usually prefer to stay with standard methods of doing things rather than using a novelty mechanism just for the sake of it. While these may make your code a few lines shorter, it does so at the expense of readilibity, as even other Python programmers may not be familiar enough to immediately understand what's going on. Python code is already very short compared to most standard programming languages so really, most of the time, there isn't any need to sacrifice readibility just to reduce your file by a couple of lines of code.

On the other hand, sometimes these can greatly simplify the solution to a problem or speed up its execution - or maybe you're just writing code for yourself and you don't care about readibility issues since no one else will be looking at it. Anyway, Python is very meticulous about introducing syntax into the language itself so even if it's new, most of the time it shouldn't be too esoteric for anyone to figure out what is happening... especially if you include a comment with a link to this document, perhaps ;)

Feel free to submit [a new issue to this project's tracker](https://github.com/tukkek/notablepython/issues/new) or a pull request if you feel like I have missed out on something. However, keep in mind that I have to draw the line between "underutilized" and "standard" Python somewhere, which is why you won't see things such as annotations or generators being discussed here, even though a lot of begginer Python coders would find them to be somewhat esoteric upon first encountering them, especially if they're coming from older languages or just now learning how to code.

This document is up-to-date with **Python 3.7**.

# Chaining comparison operators

```py
x = 5
1 < x < 10 #True
10 < x < 20 #False
x < 10 < x*10 < 100 #True
```

Taking the first comparison as an example, the way this works is equivalent to `(1 < x) and (x < 10)`. Note, however that this can create some counter-intuitive cases if abused, like the one below:

```py
True is False == False #False
```

Source: https://stackoverflow.com/a/101945

# In-place value swapping

```py
a,b = 1,2
a, b = b, a
print(a,b) # prints "2 1"
```

Simple trick but removes the need for a placeholder variable, in a fairly common scenario. Source: https://stackoverflow.com/a/102037

# Unconventional `else` blocks


```py
def does_exists_num(list, to_find):
      for num in list:
          if num == to_find:
              print("Exists!")
              break
      else: # executed if "break" above isn't reached
          print("Does not exist") 
```


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

# Acknowledgments

I'd like to thank Satwik Kansal for the [What the f\*ck, Python?](https://github.com/satwikkansal/wtfPython) project. I have started this reference a couple of times in the past but seeing how neatly and enjoyable his own documentation was, it renewed my focus to get this done and hopefully be as fun/useful to read as his own was!
