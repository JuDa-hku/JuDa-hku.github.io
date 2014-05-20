---
layout: post
title: Generalized Linear Models Base.py
categories:
- Programming
tags:
- Python
- Algorithm
- Sklearn

---
* Python tips
* Base.py of the lib
* Functions in Base


---


## Python tips
Values may be modified in the function. For mutable value, it can be changed in the function, for inmutable variable, it will not be changed in the function
{%highlight objc %}
def try_change(a,b,c):
	a=10
	b.append(42)
	c=[12]
	print(a,b,c)
x=9
y=[10]
z=[11]
try_change(x,y,z)
print(x,y,z)
{% endhighlight objc%}

A class method receives the class as implicit first argument, just like an instance method receives the instance. To declare a class method, use this idiom:
{% highlight objc %}
class C(object):
	@classmethod
	def f(cls,arg1,arg2,...):
		...
{% endhighlight objc %}
A static method does not receive an implicit first argument.To declare a static method
{% highlight objc %}
class C(object):
	@staticmethod
	def f(arg1,arg2,...):
		...
{% endhighlight objc %}


*hasattr(object, name)* The arguments are an object and a string. The result is True if the string is the name of one of the object’s attributes, False if not.


*getattr(object, name[, default])* Return the value of the named attribute of object. name must be a string. If the string is the name of one of the object’s attributes, the result is the value of that attribute. For example, getattr(x, 'foobar') is equivalent to x.foobar. If the named attribute does not exist, default is returned if provided, otherwise AttributeError is raised. *getattr(x,"output_%s" %format)* here the **out_put%s** is a variable and we can not use **.**.


*setattr(object, name, value)* This is the counterpart of getattr(). The arguments are an object, a string and an arbitrary value. The string may name an existing attribute or a new attribute. The function assigns the value to the attribute, provided the object allows it. For example, setattr(x, 'foobar', 123) is equivalent to x.foobar = 123. For example, ' 1  2   3  '.split() returns ['1', '2', '3'], and '  1  2   3  '.split(None, 1) returns ['1', '2   3  '].

     

*str.split([sep[, maxsplit]])* Return a list of the words in the string, using sep as the delimiter string. If maxsplit is given, at most maxsplit splits are done (thus, the list will have at most maxsplit+1 elements). If maxsplit is not specified or -1, then there is no limit on the number of splits (all possible splits are made).

[Inspect](https://docs.python.org/2/library/inspect.html) modulex provides several useful functions to help get information about live objects such as modules, classes, methods, functions, tracebacks, frame objects, and code objects. *inspect.getargspec(func)* Get the names and default values of a Python function’s arguments.

[\*\*kwargs](https://docs.python.org/2/tutorial/controlflow.html#keyword-arguments) You can use *\*\*kwargs* to let your functions to take an arbitrary number of keyword argument and use *args to receives a tuple containing the positional argument.
{% highlight objc %}
def print_keyword_args(*args,**kwargs):
	for arg in args:
		print arg
	keys=sorted(kwargs.keys())
	for kw in keys:
		print kw,":",kwargs[kw]
print_keyword_args("it is","me",you="happy",me="happy")
{% endhighlight objc %}


## The base.py of the lib
BaseEstimator is the base class for all estimators in scikit-learn. There is a *@classmethod* which is **_get_param_names(cls)**. Two main functions are *init=getattr(cls.__init__, 'deprecated_original', cls.__init__)* and *args,varargs,kw,default=inspect.getargspec(init)*. One function defined in the class is *get_params(self,deep=True)* which gets parameters for this estimator and **considers deprecationWarning**(I donot know why).  *set_params(self,\*\*parms)* sets parameters of the estimator on simple as well as nested case.

Other classes defined here are classifierMixin, RegressorMixin, clusterMixin, BiclusterMixin, TransformerMixin, MetaEstimatorMixin.



## Functions in Base of Generalized Linear Models
- **from __future__ import division** makes '/' support exact divide, for instance 3/4=0.75 and use '//' for dividing between intergers 3//4=0.
- **from abc import ABCMeta, abstractmethod** abc means *abstract base classes* and [here](https://docs.python.org/2/library/abc.html) for more.
- **from scipy.sparse.linalg import lsqr** to use lsqr to find a large, sparse, linear, system of equation. [Refer](http://docs.scipy.org/doc/scipy-0.13.0/reference/generated/scipy.sparse.linalg.lsqr.html)
- **import numbers** The numbers module defines a hierarchy of numeric abstract base classes which progressively define more operations, use *isinstance(x,numbers.Number)* to check whether x is number or not. [Here](https://docs.python.org/2/library/numbers.html) for more
- **from ..externals import six** six provides simple utilities for wrapping over difference between python 2 and 3. 2*3=6. [Here](https://pythonhosted.org/six/) for more.
- CSR is short for *Compressed Sparse Row* and it is a type for sparse matrix.
- **..utils import as_float_array** is a little bit hard to get. Find the utils dir first and in **__init__.py**, we find *from .validation import as_float_array* which means it is in the validation.py.

