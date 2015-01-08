---
layout: post
title: Python Tutorial
categories:
- Programming
tags:
- Python

---
* Mutable or Immutable
* Important warning
* Executing Modules as Scripts
* Private Variable and Class-local References
* Iterators and Generators

---
Website for Python learning (look at them often). [Code Like a Pythonista](http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html), [Code Style](http://docs.python-guide.org/en/latest/writing/style/) 
<!--  -->

## Mutable or Immutable
- Unlike a C string, Python strings cannot be changed. Assigning to an indexed position in the string results in an error.
- Unlike strings, it is possible to change individual elements of a list. Lists are mutable, and their elements are usually homogeneous.
- Tuples are immutable and usually contain an heterogeneous sequence of elements that are accessed via unpacking.


## Important warning
The default value in function is evaluated only once. This makes a difference when the default is a mutable object such as a list, dictionary, or instances of most classes. Compare these two functions.
{% highlight objc %}
def f(a, L=[]):
	L.append(a)
	return L
print f(1)
print f(2)

def f(a, L=None):
	if L is None:
		L = []
	#initialize a L
	L.append(a)
	return L

def f(a, L=None):
	if L is None:
		L1 = []
		L1.append(a)
		return L1
	else:
		L.append(a)
		return L
{% endhighlight objc %}

Use * to unpack arguments list.

	range(3,6)
	args = [3,6]
	range(*args)

lambda expressions are just syntactic sugar for a normal function definition.

	pairs = [(1,'one'), (2,'two'), (3,'three')]
	pairs.sort(key=lambda pair: pair[1])
	pairs

## Executing Modules as Scripts
With the *__name__* set to *__main__*, you can make the file usable as a script, because the code only runs if the module is executed as the "main" file.

	if __name__ = "__main__"
		import sys
		fib(int(sys.argv[1]))


## Private Variable and Class-local References
"Private" instance don't exist in Python. However, there is a convention that is followed by most Python code: a name prefixed with an underscore(e.g. _spam). There is a valid use-case for class-private members, any identifier of the form __spam is textually replaced with *_classname_spam* where classname is the current class name.
{% highlight objc %}
class Mapping:
	def __init__(self, iterable):
		self.itmes_list = []
		self.Mapping__update(iterable)

	def Mapping__update(self, iterable):
		for item in iterable:
			self.items_list.append(item)

class MappingSubclass(Mapping):
	MappingSubclass__update(self, keys, values):
		for item in zip(keys, values):
			self.items_list.append(item)
{% endhighlight objc %}

## Iterators and Generators
The use of iterators pervades and unifies Python. The *for* statement calls *iter()* on the container object. The function returns an iterator object that defines the method *next()* which accesses elements one at a time. When there are no more elements, *next()* raises a *StopIteration* exception.
{% highlight objc%}
class Reverse:
	def __init__(self,data):
		self.data = data
		self.index = len(data)
	def __iter__(self):
		return self
	def next(self):
		if self.index == 0:
			raise StopIteration
		self.index = self.index - 1
		return self.data[self.index]
rev = Reverse('golf')
for char in rev:
	print char
{% endhighlight objc %}	

*Generators* provide a powerful tool for creating iterators, use **yield** to return data. Each time *next()* is called, the generator resumes where it left-off.

	def reverse(data):
		for index in range(len(data)-1, -1, -1):
			yield data[index]
	for char in reverse('golf'):
		print char
