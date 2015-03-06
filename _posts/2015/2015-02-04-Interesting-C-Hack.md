---
layout: post
title: Interesting C Hack
categories:
- Programming
tags:
- C



---
* Drawback of #define
* C compile trick
* Little endian and big endian 


---
<!-- insert ![RainFall](/png/2012RainFall.png?raw=true) -->
<!-- insert hyper []() -->
<!-- insert code {%highlight python%} {%endhighlight%} -->
<!-- insert code {%highlight C++%} {%endhighlight%} -->

## Drawback of #define
{%highlight C++%}
#define MAX(a,b) (((a)>(b))?(a):(b))
int res=MAX(fib(10), factorial(40))
int larger=MAX(m++, n++)
{%endhighlight%}
This snippet call one function at least twice. If we expand larger, it add 1 to the value m or n not just once but twice. We should be careful to use define.

## C compile trick
This is the example from cs107 course. Gcc compile separates the compile and linkage process. The #include keyword is just used to announce prototype and if we miss some head file, gcc will give warnings.

{%highlight C++%}
int strlen(char*s1, char*s, int num);
#include<assert.h> //#define assert
int main(){
  int num;
  char a[4]="123";
  int length=strlen(a,(char*)&num, num);
  printf("length=%d\n", length);
  return 0;
}
{%endhighlight%}

If you really know how compile and link works, you will know the result and warnings. 

## Little endian and big endian
{%highlight C++%}
#include<stdio.h>
int main(){
  //  int i;
  short array[4];
  int i;
  for(i=0; i<=4; i++){
    array[i]=1;
  }
  *(array-1) = 1;
  printf("%d\n", &i);
  printf("%d\n", array-1);
  //for little endian structure, the lower byte is smaller and we change high byte in i.
  //i will equal to 2**16+5
  printf("%d\n", i);
  return 0;
}
{%endhighlight%}
Little endian will store least significant data in the smallest address. Big endian will store most significant data in the smallest address. For this  case, I change i in the stack by manually *(array-1)=1. The 'for' loop may cause infinite loop in the stack if i's address is just equal to &(array[4]).
