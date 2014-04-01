---
layout: post
title: Improper Use of Pointer
categories:
- Programming
tags:
- C

---
* Pointer Declaration and initialization
* Pointer Usage Issues

---

## Pointer Declaration and initialization
Consider the following declaration:
	int* ptr1, ptr2;
There is nothing wrong; however, it may not be what was intented. This declares **ptr1** as a pointer to an integer and **ptr2** as an integer.

Variables may be declared with the assistance of a directive, as shown below.
{% highlight objc %}
	#define PINT int*
	PINT ptr1,ptr2;
{% endhighlight %}
However, the result is the same problem as the previous one. A better aprproach is:
{% highlight objc %}
typedef int* PINT;
PINT ptr1,ptr2;
{% endhighlight %}

## Failure to initialize a pointer before it is used
A simple example follows where a pointer to an integer is declared but is never assigned a value before it is used:
{% highlight objc %}
int *pi;
...
printf("%d\n",*pi);
{% endhighlight %}
The variable **pi** has not been initialized and will contain garbage.

## Dealing with Uninitialized Pointers
Nothing inherent in a pointer tells us whether it is valid. Thus, we cannot simply examine its contents to determine whether it is valid. However, three approaches are used to deal with uninitialized pointers:

- Always initialize a pointer with NULL
- Use the assert function
- Use third party tools

{% highlight objc %}
int *pi=NULL;
...
if(pi==NULL){
	//pi should not be dereferenced
}else{
	//pi can be used
}
{% endhighlight %}
The **assert** function can also be used to test for null pointer values. The **pi** variable is tested for a null vlaue. If the expression is false, then the program terminates.
	#include<assert.h>
	assert(pi!=NULL);

## Pointer Usage Issues
We will examine misuse of the dereference operator and array subscripts. Buffer overflow can happen by:

- Not checking the index values used when accessing an array's elements
- Not being careful when performing pointer arithmetic with array pointers
- Using functions such as strcpy and strcat improperly
- Using functions such as **gets** to read in a string from standard input

Always check the return value when using a **malloc** type function.
{% highlight objc %}
float *vecotr=malloc(20*sizeof(float));
if(vecotr==NULL){
	//malloc failed to allocate memory
}else{
	//processor vector
}
{% endhighlight %}
Misuse of the Dereference Operation:
	int num;
	int *pi=&num;
Another seemingly equivalent declaration:
	int num;
	int *pi;
	*pi=&num;
However, this is not corret. \* is the dereference operator on the last line.

**Accessing Memory Outside the Bounds of an Array** happens. Nothing can prevent a program from accessing memory outside of the space allocated for an array.
{% highlight objc %}
char firstName[8]="1234567";
char middleName[8]="1234567";
char lastName[8]="1234567";
middleName[-2]='X';
middleName[0]='X';
middleName[10]='X';
{% endhighlight %}
**Calculating the Array Size Incorrectly**. When passing an array to a function, always pass the size of the array at the same time. This information will help the function avoid exceeding the bounds of the array.
{% highlight objc %}
void replace(char buffer[],char replacement,size_t size){
size_t count=0;
while(*buffer!=NUL&&count++<size){
	*buffer=replacement;
	buffer++;
}
}
char name[8];
strcpy(name,"ALEXANDER");
replace(name,'+',sizeof(name));
printf("%s\n",name);
{% endhighlight %}
we will get **++++++++R**, only eight + were added to the array. While the **strcpy** function permitted buffer overflow, the **replace** did not. Functions like **strcpy** taht do not pass buffer's size should be used with caution.

**Pointer arithmetic** should only be used with arrays. Because arrays are guaranteed to be allocated in a contiguous block of memory, pointer arithmetic will result in a valid offset. This is illustrated with the following structure. The **name** field is allocated 10 bytes, and is followed by an integer. However, since the integer will be aligned on a four-byte boundary, there will be a gap between the two fields.
{% highlight objc %}
typedef struct _employee{
char name[10];
int age;
} Employee;
Employee employee;
char *ptr=employee.name;
ptr+=sizeof(employee.name);
{% endhighlight %}

