---
layout: post
title: Introducation to Pointers
categories:
- Programming
tags:
- C
---
* Why Pointers
* How to Read a Declaration
* The Concept of Null
* Pointer to void
* Predefined Pointer-Related Types
* Multiple Levels of Indirection
* Constants and Pointers
* Dynamic Memory Allocation

---

## Introducation.
[Understanding and Using C pointers](http://www.amazon.com/Understanding-Using-Pointers-Richard-Reese/dp/1449344186)

## Why Pointers
Pointers have several uses, including:

+ Creating fast and efficient code
+ Providing a convenient means for addressing many types of problems
+ Supporting dynamic memory allocation
+ Making expressions compact and succinct
+ Providing the ability to pass data structures by pointer without incurring a large overhead
+ Protecting data passed as a parameter to a function

## How to Read a Declaration
Reading the declaration backward allows us to progressively understand the declaration.

+ pci is a variable const int \***pci**;
+ pci is a pointer variable const int **\*pci** ;
+ pci is a pointer variable to an integer const **int \*pci**;
+ pci is a pointer variable to a constant integer **const int \*pci**

## The Concept of Null
Confusion can occur because we often deal with several similar, yet distinct concepts, including:

- The null concept
- The null pointer constant
- The NULL macro
- The ASCII NUL
- A null string
- The null statement

The **null concept** refers to the idea that a pointer can hold a special value that is not equal to another pointer. It does not point to any area of memory. Two null pointers will always be equal to each other. The null concept is an abstraction supported by *the null pointer constant*.

The **NULL macro** is a constant integer zero cast to a pointer to void. In many libraries, it is defined as follows:
	#define NULL ((void *)0)
This is what we typically think of as a null pointer.

The **ASCII NUL** is defined as a byte containing all zeros. However, this is not the same as a null pointer. A string in C is represented as a sequence of characters terminated by a zero value. The null string is an empty string and does not contain any characters. Finally, **the null statement** consists of a statement with a single semicolon(;).

## Pointer to void
A pointer to void is a general-purpose pointer used to hold references to any **data** type.

- A pointer to void will have the same representation and memory alignment as a pointer to char.
- A pointer to void will never be equal to another pointer. However, two void pointers assigned a NULL value will be equal.

Any pointer can be assigned to a pointer to void. It can then be cast back to its original pointer type.
{% highlight objc %}
int num;
int *pi=num;
void* pv=pi;
pi=(int*)pv;
printf("Value of pi: %p\n",pi)
{% endhighlight %}
Pointers to void are used for data pointers, not function pointers.
If a pointer is declared as global or static, it is initialized to NULL when the program starts.

## Predefined Pointer-Related Types
Four predefined types are frequently used when working with pointer. They include:

- size_t
  - Created to provide a safe type for sizes
- ptrdiff_t
  - Created to handle pointer arithmetic
- intptr_t and uintprt_t
  - Used for storing pointer addresses

The size_t type is used as the return type for the **sizeof** operator and as the argument to many functions, including malloc and strlen, among others. The declaration of size_t is implementation-specific. It is found in one or more standard headers, such as stdio.h and stdlib.h and it is typically defined as follows:
{% highlight objc %}
#ifndef _SIZE_T
#define _SIZE_T
typedef unsigned int size_t;
#endif
{% endhighlight %}
Always use the **sizeof** operator when the size of a pointer is needed.
{% highlight objc %}
print("size of *char: %d\n",sizeof(char*))
{% endhighlight %}
*Using intptr_t and unitptr_t*
The types of **intptr_t** and **unitptr_t** are used for storing pointer addresses. They provid a portable and safe way of declaring pointers. They are useful for converting pointers to their integer representation. **unitptr_t** is the unsigned version of **intptr_t**. For most operations intptr_t is preferred. The type uintptr_t is not as flexible as intptr_t. The following illustrates how to use.
{% highlight objc %}
int num;
intptr_t *pi=&num;
{% endhighlight %}
## Multiple Levels of Indirection
It is not uncommon to see a variable declared as a point to a pointer, sometimes called a *double pointer*. A good example of this is when program arguments are passed to the main function using the traditionally named argc and argv parameters. The example here uses two arrays. The first one is used to hold a list of book titles, the second *bestbooks* array hold the address of a title in the *titles* array. The *bestbooks* array will need to be declared as a pointer to a pointer to a **char**.
{% highlight objc %}
char *titles[]={"A Tale of Two Cities",
	"Wuthering Heights","Don Quixote",
	"Odyssey","Moby-dick"};
char **bestBooks[3];
bestBooks[0]=&titles[0];
{% endhighlight %}
This will avoid having to duplicate memory for each title and results in a single location for titles. If a tile needs to be changed, then the change will only have to be performed in one location.Of course, using too many levels of indirection can be confusing and hard to maintain.

## Constants and Pointers
The declaration of **pci** as a pointer to a constant integer means:

- **pci** can be assigned to point to different constant integers
- **pci** can be assigned to point to different nonconstant integers
- **pci** can be dereferenced for reading purposes
- **pci** cannot be dereferenced to change what it points to

Here the order of the type and *const* keyword is not important. The following are equivalent:
{% highlight objc %}
const int *pci
int const *pci
{% endhighlight %}
Constant pointer to nonconstants
{% highlight objc %}
int num;
int *const cpi=&num;
{% endhighlight %}

- *cpi* must be initialized to a nonconastant variable
- *cpi* cannot be modified
- The data pointed to by cpi can be modified

If we use the code
{% highlight objc %}
const int limit=500
int *const cpi=&limit
{% endhighlight %}
The warning will appear *initialization discards qualifiers from pointer target type*, If **cpi** referenced the constant **limit**, the constant could be modified. This is not desirable. We generally prefer constants to remain constant.

Pointers to constants can also have multiple levels of indirection. Reading complex declarations from right to left helps clarify these types of declarations
{% highlight objc %}
const int limit=500;
const int * const cpci=&limit
const int * const * pcpci=&cpci
// A pointer to a constant pointer to a constant
{% endhighlight %}

## Dynamic Memory Allocation
The basic steps used for dynamic memory allocation in C are:

- Use a malloc type function to allocate memory
- Use this memory to support the application
- Deallocate the memory using the free function

{% highlight objc %}
int *pi=(int*) malloc(sizeof(int))
*pi=5
printf("*pi: %d\n",*pi)
free(pi)
{% endhighlight %}
Each time the malloc function (or similar function) is called, a corresponding call to the free function must be made when the application is done with the memory to avoid memory leaks.
{% highlight objc %}
char *chunk
while(1){
chunk=(char*) malloc(10000);
printf("Allocating\n");
}
{% endhighlight %}
Losing the address example
{% highlight objc %}
int *pi=(int*) malloc(sizeof(int))
*pi=5
pi=(int*) malloc(sizeof(int))
{% endhighlight %}
Dynamic memory allocation functions.

- malloc Allocates memory from the heap
- realloc Reallocates memory to a larger or smaller amount based on a previously allocated block of memory
- calloc Allocates and zeros out memory from the heap
- free Returns a block of memory to the heap






