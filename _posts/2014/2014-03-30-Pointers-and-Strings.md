---
layout: post
title: Pointers and Strings
categories:
- Programming
tags:
- C

---
* The String Literal Pool
* String Initialization
* Passing Strings
* Returning Strings
* Function Pointers and Strings

---

A string is a sequence of characters terminated with the ASCII NUL character. The ASCII character NUL is represented as \0. Strings are commonly stored in arrays or in memory allocated from the heap. However, not all arrays of characters are strings. An array of **char** may not contain the **NUL** character. Remember that **NULL** and **NUL** are different. **NULL** is used as a special pointer and is typically defined as **((void*)0)**. **NUL** is a char and is defined as '\0'.

Character constants are character sequences enclosed in single quotes. Normally, they consist of a single character but can contain more than one character. In C, they are of type int. This is demonstrated as follows:
{% highlight objc %}
printf("%d\n",sizeof(char));
printf("%d\n",sizeof('a'));
{% endhighlight %}
When executed, the size of char will be 1 while the character literal's size will be 4.

## The String Literal Pool
When literals are defined they are frequently assigned to a literal pool. This area of memory holds the character sequence making up a string. When a literal is used more than once, there is normally only single copy of the string in the string literal pool. In most compilers, a string literal is treated as a constant. It is not possible to modify the string. GCC will throw a warning when we use
	char *tabHeader="Sound";
Warning: transforming a constant string to char *. Use **constant char\*** to solve the problem, **constant char\* tabHeader="Sound"**.

## String Initialization
An array of **char** can be initialized using the initialization operation and an array can also be initialized using strcpy function.
{% highlight objc %}
char header[]="Media Player";
char header[13];
strcpy(header,"media Player");
{% endhighlight %}
Using dynamic memory allocation provides flexibility and potentially allows the memory to stay around longer.
{% highlight objc %}
char *header=(char*) malloc(strlen("Media Player")+1);
strcpy(header,"Media Player");
{% endhighlight %}
When determining the length of a string to be used with the malloc function:

- Always remember to add one for the NUL terminator.
- Don't use **sizeof** operator. Instead, use the strlen function to determine the length of an existing string. The sizeof operator will return the size of an array or pointer, not the length of the string.

Attempting to initialize a pointer to a char with a character literal will not work. Since a character literal is of type int, we would be trying to assign an integer to a character pointer. This will frequently cause the application to terminate when the pointer is dereferenced:
	char* perfix='+'; //Illegal
A valid approach using the **malloc** function follows:
{% highlight objc %}
prefix=(char*)malloc(2);
*prefix='+';
*(prefix+1)=0;
{% endhighlight %}

## Passing Strings
Passing a string is simple enough. In the function call, use an expression that evaluates to the address of a **char**. In the parameter list, declare the parameter as a pointer to a **char**. We use parentheses to force the post increment operator to execute first, incrementing the pointer.
{% highlight objc %}
size_t stringLength(char* string){
	size_t length=0;
	while(*(string++)){
	length++;
	}
	return length;
}
{% endhighlight %}
Passing a pointer to a string as a constant **char** is a very common and useful technique. It passes the string using a pointer, and at the same time prevents the string being passed from being modified.
{% highlight objc %}
size_t stringLength(const char* string){
	size_t length=0;
	while(*(string++)){
	length++;
	}
	return length;
}
{% endhighlight %}

## Returning Strings
When a function returns a string, it returns the address of the string. The main concern is to return a valid string address. To do this, we can return a regerence to: A literal. Dynamically allocated memory. A local string variable.

If a string needs to be returned from a function, the memory for the string can be allocated from the heap and its address can be returned.
{% highlight objc %}
char *blanks(int number){
	char* spaces=(char*)malloc(number+1);
	int i;
	for(i=0;i<number;i++){
		spaces[i]=' ';
}
	spaces[number]='\0';
	return spaces;
}
char *tmp=blanks(3);
printf("[%s]\n",tmp);
free(tmp);
{% endhighlight %}
It is a safer way to return the memory we used.

Returning the address of a local string will be a problem since the memory will be corrupted when it is overwritten by another stack frame.

## Function Pointers and Strings
Function pointers can be a flexible means of controlling how a program executes.
{% highlight objc %}
#include<stdio.h>
#include<stdlib.h>
#include<ctype.h>
#include<string.h>
char * stringToLower(const char* string){
  char* tmp=(char*) malloc(strlen(string)+1);
  char *start=tmp;
  while(*string!=0){
    *tmp++=tolower(*string++);
  }
  *tmp=0;
  return start;
}
int compare(const char*s1,const char* s2){
  return strcmp(s1,s2);
}
int compareIgnoreCase(const char* s1,const char *s2){
  char* t1=stringToLower(s1);
  char *t2=stringToLower(s2);
  int result=strcmp(t1,t2);
  free(t1);
  free(t2);
  return result;
}
typedef int (fptrOperation) (const char*, const char*);
void sort(const char *array[],int size, fptrOperation operation){
  int swap=1;
  while(swap){
    swap=0;
    for(int i=0;i<size-1;i++){
      if(operation(array[i],array[i+1])>0){
	swap=1;
	const char *tmp=array[i];
	array[i]=array[i+1];
	array[i+1]=tmp;
      }
    }
  }
}
void displayNames(const char* names[],int size){
  for(int i=0;i<size;i++){
    printf("%s  ",names[i]);
  }
  printf("\n");
}
int main()
{
 const char* names[]={"bob","TED","Carol","Alice","alice"};
  sort(names,5,compare);
  displayNames(names,5);
  sort(names,5,compareIgnoreCase);
  displayNames(names,5);
}
{% endhighlight %}
This makes the **sort** function much more flexible. We can now devise and pass as a simple or complex an opertaion as we want to control the sort without having to write different **sort** functions for different sorting needs.

