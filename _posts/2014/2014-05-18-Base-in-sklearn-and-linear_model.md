---
layout: post
title: Base.py in sklearn and Base.py in linear_model
categories:
- Programming
tags:
- Python
- Algorithm
- Scikit

---
* Python tips
* sklearn/base.py
* sklearn/linear_model/base.py


---


## Python tips
Values may be modified in the function. For mutable value, it can be changed in the function, for inmutable variable, it will not be changed in the function
{%highlight python %}
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
{% endhighlight python%}

A class method receives the class as implicit first argument, just like an instance method receives the instance. To declare a class method, use this idiom:
{% highlight python %}
class C(object):
	@classmethod
	def f(cls,arg1,arg2,...):
		...
{% endhighlight python %}
A static method does not receive an implicit first argument.To declare a static method
{% highlight python %}
class C(object):
	@staticmethod
	def f(arg1,arg2,...):
		...
{% endhighlight python %}


*hasattr(object, name)* The arguments are an object and a string. The result is True if the string is the name of one of the object’s attributes, False if not.


*getattr(object, name[, default])* Return the value of the named attribute of object. name must be a string. If the string is the name of one of the object’s attributes, the result is the value of that attribute. For example, getattr(x, 'foobar') is equivalent to x.foobar. If the named attribute does not exist, default is returned if provided, otherwise AttributeError is raised. *getattr(x,"output_%s" %format)* here the **out_put%s** is a variable and we can not use **.**.


*setattr(object, name, value)* This is the counterpart of getattr(). The arguments are an object, a string and an arbitrary value. The string may name an existing attribute or a new attribute. The function assigns the value to the attribute, provided the object allows it. For example, setattr(x, 'foobar', 123) is equivalent to x.foobar = 123. For example, ' 1  2   3  '.split() returns ['1', '2', '3'], and '  1  2   3  '.split(None, 1) returns ['1', '2   3  '].

     

*str.split([sep[, maxsplit]])* Return a list of the words in the string, using sep as the delimiter string. If maxsplit is given, at most maxsplit splits are done (thus, the list will have at most maxsplit+1 elements). If maxsplit is not specified or -1, then there is no limit on the number of splits (all possible splits are made).

[Inspect](https://docs.python.org/2/library/inspect.html) modulex provides several useful functions to help get information about live objects such as modules, classes, methods, functions, tracebacks, frame objects, and code objects. *inspect.getargspec(func)* Get the names and default values of a Python function’s arguments.

[\*\*kwargs](https://docs.python.org/2/tutorial/controlflow.html#keyword-arguments) You can use *\*\*kwargs* to let your functions to take an arbitrary number of keyword argument and use *args to receives a tuple containing the positional argument.
{% highlight python %}
def print_keyword_args(*args,**kwargs):
	for arg in args:
		print arg
	keys=sorted(kwargs.keys())
	for kw in keys:
		print kw,":",kwargs[kw]
print_keyword_args("it is","me",you="happy",me="happy")
{% endhighlight python %}

[Staticmethod(function)](https://docs.python.org/2/library/functions.html#staticmethod) return a static method for function.A static method does not receive an implicit first argument. To declare a static method, use this idiom:
 {%highlight python %}
    class C(object):
        @staticmethod
        def f(arg1, arg2, ...):
            ...
{% endhighlight python %}
It can be called either on the class (such as C.f()) or on an instance (such as C().f()). The instance is ignored except for its class.

[Classmethod(function)](https://docs.python.org/2/library/functions.html#classmethod) return a class method for function. A class method receives the class as implicit first argument, just like an instance method receives the instance. To declare a class method, use this idiom:
{% highlight python%}
    class C(object):
        @classmethod
        def f(cls, arg1, arg2, ...):
            ...
{% endhighlight %}
It can be called either on the class (such as C.f()) or on an instance (such as C().f()). The instance is ignored except for its class. If a class method is called for a derived class, the derived class object is passed as the implied first argument. **Pay** attention to the difference with *staticmethod*.

@classmethod means: when this method is called, we pass the class as the first argument instead of the instance of that class (as we normally do with methods). This means you can use the class and its properties inside that method rather than a particular instance.

@staticmethod means: when this method is called, we don't pass an instance of the class to it (as we normally do with methods). This means you can put a function inside a class but you can't access the instance of that class (this is useful when your method does not use the instance).


With_metaclass() is a utility class factory function provided by the six library to make it easier to develop code for both Python 2 and 3.It creates a base class with the specified meta class for you, compatible with the version of Python you are running the code on.
    Create a new class with base class base and metaclass metaclass. This is designed to be used in class declarations like this:
    from six import with_metaclass
    class Meta(type):
        pass
    class Base(object):
        pass
    class MyClass(with_metaclass(Meta, Base)):
        pass
This is needed because the syntax to attach a metaclass changed between Python 2 and 3:
{%highlight python %}
Python 2:
class MyClass(object):
    __metaclass__ = Meta
Python 3:
class MyClass(metaclass=Meta):
    pass
{% endhighlight python %}
The with_metaclass() function makes use of the fact that metaclasses are a) inherited by subclasses, and b) a metaclass can be used to generate new classes; it effectively creates a new base class by using the metaclass as a factory to generate an empty class:

numpy.exp(prob,prob) is different with prob=numpy.exp(prob).

Function name can also be regarded as variable in functions.
{% highlight python %}
def func(a=repr):
b=a(123)
print b
func()
def func1(x):
return x+1
func(func1)
{% endhighlight python %}

why we need .copy()? Assignment statements in Python do not copy objects, they create bindings between a target and an object. For collections that are mutable or contain mutable items, a copy is needed so one can change one copy without changing the other.

collections provides high performance containers. [from collections import defaultdict](https://docs.python.org/2/library/collections.html?highlight=defaultdict#collections.defaultdict), *defaultdict* object is a new dictionary-like object.
{% highlight python %}
 s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
 d = defaultdict(list)
 for k, v in s:
     d[k].append(v)
 d.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
{% endhighlight python %}
Setting the default_factory to int makes the defaultdict useful for counting.
{% highlight python %}
s='mississippi'
d=default(int)
for k in s:
	d[k]+=1
d.items()	
{% endhighlight python %}

Why run the code **import os**, **print os.path.realpath(__file__)** directly does not work?

You can't directly determine the location of the main script being executed. After all, sometimes the script didn't come from a file at all. For example, it could come from the interactive interpreter or dynamically generated code stored only in memory. However, you can reliably determine the location of a module, since modules are always loaded from a file. If you create a module with the following code and put it in the same directory as your main script, then the main script can import the module and use that to locate itself.

## sklearn/base.py
**clone(estimator, safe=True)** constructs a new estimator with the same
parameters and yields a new estimator with the same parameters that
has not been fit on any data. The function is not
esay. \_pprint(params, offset=0, printer=repr) print the dictionary
'params'.

BaseEstimator is the base class for all estimators in scikit-learn. There is a *@classmethod* which is **_get_param_names(cls)**. Two main functions are *init=getattr(cls.__init__, 'deprecated_original', cls.__init__)* and *args,varargs,kw,default=inspect.getargspec(init)*. One function defined in the class is *get_params(self,deep=True)* which gets parameters for this estimator and **considers deprecationWarning**(I donot know why).  *set_params(self,\*\*parms)* sets parameters of the estimator on simple as well as nested case.

- class ClassifierMixin(object): Mixin class for all classifiers, def score(self, X, y):.
- class RegressorMixin(object): Mixin class for all regression estimators, def score(self, X, y):.
- class ClusterMixin(object): Mixin class for all cluster estimators, def fit_predict(self, X, y=None):.
- class BiclusterMixin(object): Mixin class for all bicluster estimators,(define @proerty def biclusters_(self) to return self.rows_, self.columns_), （def get_indices(self, i) to return the row and column indices of bicluster 'i'), (def get_shape(self, i) return shape of bicluster 'i')
- class TransformerMixin(object): Mixin class for all transformers. (def fit_transform(self, X, y=None, **fit_params) to fit transformer to X and y with optional parameters fit_params and returns a transformed version of X)
- def _get_sub_estimator(estimator): return the final estimator if there is any.
- def is_classifier(estimator): return ture if the given estimator is (probably) a classifier.





## sklearn/linear_model/base.py
- **from __future__ import division** makes '/' support exact divide, for instance 3/4=0.75 and use '//' for dividing between intergers 3//4=0.
- **from abc import ABCMeta, abstractmethod** abc means *abstract base classes* and [here](https://docs.python.org/2/library/abc.html) for more.
- **from scipy.sparse.linalg import lsqr** to use lsqr to find a large, sparse, linear, system of equation. [Refer](http://docs.scipy.org/doc/scipy-0.13.0/reference/generated/scipy.sparse.linalg.lsqr.html)
- **import numbers** The numbers module defines a hierarchy of numeric abstract base classes which progressively define more operations, use *isinstance(x,numbers.Number)* to check whether x is number or not. [Here](https://docs.python.org/2/library/numbers.html) for more
- **from ..externals import six** six provides simple utilities for wrapping over difference between python 2 and 3. 2*3=6. [Here](https://pythonhosted.org/six/) for more.
- CSR is short for *Compressed Sparse Row* and it is a type for sparse matrix.
- **..utils import as_float_array, atleast2d_or_csr, safe_asarray** is a little bit hard to get. Find the utils dir first and in **__init__.py**, we find *from .validation import as_float_array* which means it is in the validation.py.
- [numpy.linalg.lstsq(a, b, rcond=-1)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.linalg.lstsq.html) return the least-squares solution to a linear matrix equation.

**class LinearModel(six.with_metaclass(ABCMeta, BaseEstimator))** in the base.py which include the *@abstractmethod* *fit* in it. For concrete class that inherits from the class **LinearModel**, the concrete provides an implementation of the abstract method. 

**class LinearClassifierMixin(ClassifierMixin)** includes

- *decision_function(self,X)* to predict confidence scores for samples. The confidence score for a sample is the signed distance of that sample to the hyperplane.
- *predict(self,X)* to predict class labels for samples in X.
- *_predict_proba_lr(self,X)* probability estimation for logistic regression.

**class SparseCoefMixin(object)** is the mixin for converting coef_ to and from CSR format. L1-regularizing estimators should inherit this.

- *densify(self):* convert coefficient matrix to dense array format
- *sparsify(self):* convert coefficient matrix to sparse format

**class LinearRegression(LinearModel, RegressorMixin):** for ordinary least squares Linear Regression.

- *__init__(self, fit_intercept=True, normalize=False, copy_X=True)*
- *fit(self, X, y, n_jobs=1)* return a instance of self. n_jobs: The number of jobs to use for the computation.

*_pre_fit(X,y,Xy,precompute,normalize,fit_intercept,copy)*
