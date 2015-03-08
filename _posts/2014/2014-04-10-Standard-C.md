---
layout: post
title: MakeFile and Standard C Lib 
categories:
- Programming
tags:
- C
- Lib

---
* MakeFile
* Assert Lib
* Ctype Lib

---

Basically, the book [**The Standard C Library**](http://www.amazon.com/The-Standard-Library-P-J-Plauger/dp/0131315099) and [wiki](http://en.wikipedia.org/wiki/C_standard_library) introduce the standard C lib and how to implement these standard libs. Here I choose to two libs as examples to introduce. We can use the same way to test the code in the book.

## MakeFile
If we want to compile lots of files at the same time. To link them one by one is not a good idea, especially when we update some of them and the relationship between is complex. To write a **makefile** for the whole project is a good idea. A simple makefile is given below.

{% highlight objc %}
objects=dependence0 dependence1
result:  ${objects}
	gcc -o result ${objects}
dependence0: dependence0.0 dependence 0.1
	gcc -c dependence0.0 dependence 0.1
dependence1: dependence1.0 dependence 1.1
	gcc -c dependence1.0 dependence 1.1
clean:
	rm  result dependence0 dependence1
{% endhighlight %}
We always name it makefile and just need one for our project (Obvious). Type in the *make* in the shell and it will run and we can also type **make clean** to run the **rm result dependence0 dependence1**. In the script, if it find the file that lies after **:** does not update, it will not run the script, for instance it will not run the clean automatically because there is not file after clean.

## Assert.h
Assert.h is a header file in the standard library of the C programming language that defines the C preprocessor macro assert(). The macro implements an assertion, which can be used to verify assumptions made by the program and print a diagnostic message if this assumption is false. When executed, if the expression is false, assert() writes information about the call that failed on stderr and then calls abort(). The information it writes to stderr includes:

- The source filename (the predefined macro __FILE__)
- The source line number (the predefined macro __LINE__)
- The source function (the predefined identifier __func__) (added in C99)
- The text of expression that evaluated to 0 [1]

Example output of a program compiled on Linux:

- program: program.c:5: main: Assertion `a != 1' failed.
- Abort (core dumped)

Programmers can eliminate the assertions just by recompiling the program, without changing the source code: if the macro NDEBUG is defined before the inclusion of <assert.h>, the assert() macro is defined simply as:
	#deinfe assert(ignore) ((void) 0)

The test code is easy to write, for the file in the book, I write a makefile for it.

{% highlight objc %}
objects=tassert.o xassert.o
tassert:  ${objects}
	gcc -o tassert ${objects}
xassert.o:xassert.c assert.h
	gcc -c xassert.c 
tassert.o:tassert.c assert.h
	gcc -c tassert.c 
clean:
	rm  tassert.o xassert.o tassert
{% endhighlight %}

## Ctype.h
Character handling has been important since the earliest days of C. In the ASCII character set the following test identifies a letter:

	if ('A' <= c && c <= 'Z' || 'a' <= c && c <= 'z')
	
However, this does not work for other character sets such as *EBCDIC*. Also, programmers may write this kind of code serveral times, which slows comprehension and increases the chance for errors so it is replaced by the functions in <ctype.h>.

Unlike the above example, the character classification routines are not written as comparison tests. In most C libraries, they are written as static table lookups instead of macros or functions. If we use the below method:

	#define isdigit(x) ((x) >= '0' && (x) <= '9')
	
Obviously, when we use **isdigit(SomeProgram(x))**, the program will run twice.
