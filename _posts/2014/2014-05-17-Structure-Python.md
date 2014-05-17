---
layout: post
title: Structure Python
categories:
- Programming
tags:
- Python
- Style

---
* Structure is important
* Modules
* Packages
* OO programming
* Decorators
* Dynamic typing
* Mutable and immutable types

---

## Structure is important
[Here](http://docs.python-guide.org/en/latest/writing/structure/) for more.Due to the way imports and modules are handled, it is relatively easy to structure a Python project but it is also easy to do it poorly. Some poorly structured project include:

- Multiple and messy circular dependencies.
- Hidden coupling.
- Heavy usage of global state or context.
- Spaghetti code: multiple pages of nested if clauses and for loops with a log of copy-pasted procedural code and no proper segmentation.
- Ravioli code: it consists of hundreds of similar little pieces of logic.


## Modules
Python modules are one of the main abstraction layers avavilable and the most natural one. To keep in line with the syle guide, keep module names short, lowercase, and be sure to avoid using special symbols like the dot(.) or question mark(?). A file name like **my.spam.py** is one you should avoid! In the case of this, python expects to find a *spam.py* file in a folder name my which is not the case. If you'd like, you could name it as **my_spam.py** but even our friend the underscore should not be seen often in module names.

The **import modu** statement will look for the proper file which is *modu.py* in the same directory as the caller if it exists. If it is is not found, the Python will search for it in the "path". Once **modu.py** is found, the Python interpreter will execute the module in an isolated scope. Any top-level statement in **modu.py** will be executed, including other imports if any. Function and class definitions are stored in the module's dictionary.

It is possible to use special syntax of the statement **from modu import ***. This is generally considered bad practice. **Using import * makes code harder to read and makes dependencies less clear**. Using **from modu import func** is a way to pinpoint the function you want to import and put it in the global namespace.
{% highlight objc %}
	[...]
	from modu import *
	[...]
	x=sqrt(4) #is sqrt part of modu? A builtin? Defined above?
	#This is bad

	[...]
	from modu import sqrt
	[...]
	x=sqrt(4) #sqrt may part of modu if not redefined.
	#This is better

	[...]
	import modu
	[...]
	x=modu.sqrt(4) #sqrt is visibly part of modu's namespace
{% endhighlight objc %}

##Packages
Any directory with an **__init__.py** file is considered a Python package. The different modules in the package are imported in a similar manner as plain modules, but with a special behavior for **__init__.py** file, which is used to gather all package-wide definitions.

A file *modu.py* in the directory *pack/* is imported with **import pack.modu** (why we do not use dot to name .py file). A commonly seen issue is to add too much code to *__init.py__* files. When the project complexity grows, there may be sub-package or sub-sub-packages in the dir structure and then, importing a single item from a sub-sub-package will require executing all *__init__.py*

Leaving an *__init__.py* file empty is considered normal and even a good practice. Lastly, a convenient syntax is available for importing deeply nested packages: **import very.deep.module as mod**.

## OO Programming
Everything is an object in Python. However, unlike Java, Python does not impose object-oriented programming as the main programming paradigm. There are some reasons to avoid unnecessary object-orientation. Defining custom classes is useful when we want to glue together some state and some functionality. The problem, as pointed out by the discussions about functional programming, comes from the “state” part of the equation.

Another way to say the same thing is to suggest using functions and procedures with as few implicit contexts and side-effects as possible. A function’s implicit context is made up of any of the global variables or items in the persistence layer that are accessed from within the function. Side-effects are the changes that a function makes to its implicit context. If a function saves or deletes data in a global variable or in the persistence layer, it is said to have a side-effect.


-    Pure functions are deterministic: given a fixed input, the output will always be the same.
-    Pure functions are much easier to change or replace if they need to be refactored or optimized.
-    Pure functions are easier to test with unit-tests: There is less need for complex context setup and data cleaning afterwards.
-    Pure functions are easier to manipulate, decorate, and pass-around.


##Decorators
The Python language provides a simple yet powerful syntax called ‘decorators’. A decorator is a function or a class that wraps (or decorates) a function or a method. The ‘decorated’ function or method will replace the original ‘undecorated’ function or method. Because functions are first-class objects in Python, it can be done ‘manually’, but using the @decorator syntax is clearer and thus preferred.
{% highlight objc %}
def foo():
    # do something
def decorator(func):
    # manipulate func
    return func
foo = decorator(foo)  # Manually decorate

@decorator
def bar():
    # Do something
# bar() is decorated
{% endhighlight objc %}

## Dynamic typing
The dynamic typing of Python is often considered to be a weakness.
{% highlight objc %}
a=1
a="abc"
items='a b c'
items=items.split(' ')
{% endhighlight objc %}
No efficiency gain when reusing names: Python will still create new objects.

## Mutable and immutable types
The most important part here is *mutable* object can not be used as dictionary keys, for example list.

	my_list=[1,2,3]
	my_list[0]=4
	print my_list

Using mutable types that are mutable in nature and immutable types for things fixed in nature. Attention: strings are immutable, it will be inefficient to get a new string from the original one.
{% highlight objc %}
	nums=" "
	for n in range(20):
		nums+=str(n)
	print nums #bad 

	nums=[]
	for n in range(20):
		nums.append(str(n))
	print " ".join(nums) #more efficient

	nums=[str(n) for n in range(20)]
	print "".join(nums) #best

{% endhighlight objc %}
