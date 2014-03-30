---
layout: post
title: Pointers and Arrays
categories:
- Programming
tags:
- C
---
* Differences Between Arrays and Pointers
* Using malloc to Create a One-Dimensional Array
* Using the realloc Function to Resize an Array
* Using Pointer Notation
* Dynamically Allocating a Two-Dimensional Array

---

A common misconception is that an array and a pointer are completely interchangeable. For example, although the name of an array used by itself will return the array's address, we cannot use the name by itself as the target of an assignment.

## Differences Between Arrays and Pointers
There are serveral differencs between the use of arrays and the use of pointers to arrays.
{% highlight objc %}
int vecotr[5]={1,2,3,4,5};
int *pv=vector;
{% endhighlight %}
The notation **vector[i]** generates machine code that starts at location *vecotr*, moves i positions from this location and uses its content. The notation **vector+i** generates machine code that starts at location vector, adds i to the address, and then uses the contents at that address. While the result is the same, the generated machine code is different. This difference is rarely of significance to most programmers.

There is a difference when the **size of** operator is appllied to an array and to a pointer to the same array. Applying the **size of** to **vector** will return 20, the number of bytess alllocated to the array. Applying the **size of** operator against **pv** will return 4, the pointer's size.

Consider the following:
{% highlight objc %}
pv=pv+1;
vector=vector+1; //synatx error
{% endhighlight %}
We cannot modify **vector**, only its contents. The pointer **pv** is an **lvalue**, it is ok to modify the **pv**.

## Using malloc to Create a One-Dimensional Array
This is a example.
{% highlight objc %}
int *pv=(int*) malloc(5*sizeof(int));
for(int i=0;i<5;i++){
	pv[i]=i+1;
}
{% endhighlight %}

## Using the realloc Function to Resize an Array
To illustrate the **realloc function**, we will implement a function to read in characters from standard input and assign them to a buffer. The buffer will contain all of the characters read in except for a terminating return character. Since we do not know how many characters the user will input, we do not know how long the buffer should be. We will use the **realloc** function to allocate additional space by a fixed increment amount.
{% highlight objc %}
char* getLine(void){
	const size_t sizeIncrement=10;
	char* buffer=malloc(sizeIncrement);
	char* currentPosition=buffer;
	size_t maximumLength=sizeIncrement;
	size_t length=0;
	int character;
	if(currenPosition==NULL) {return NULL;}
	while(1){
	character=fgetc(stdin);
	if(character=='\n') {break;}
	if(++length>=maximumLength){
		char *newBuffer=realloc(buffer,maximumLength+=sizeIncrement);
		if(newBuffer==NULL){
			free(buffer);
			return NULL;
		}
		currentPoision=newBuffer+(currentPosition-buffer);
		buffer=newBuffer;
	}
	*currentPosition++=character;	
	}
	return buffer;
}	
{% endhighlight %}
If the **malloc** is unable to allocate memory, the first if statement will force the function to return **NULL**. The **realloc** function creats anew block of memory. This block is **sizeIncrement** bytes larger than the old one. If it is unable to allocate memory, we free up the existing allocated memory and force the function to return **NULL**.

The realloc function can also be used to decrease the amount of space used by a pointer.
{% highlight objc %}
char* trim(char* phrase){
  char*old=phrase ;
  char* new=phrase;
  while(*old==' '){
    old++;
  }
  while(*old){
    *(new++)=*(old++);
  }
  *new=0;
  return (char*) realloc(phrase,strlen(phrase)+1);
x}
int main()
{
  char* buffer=(char*)malloc(strlen("   cat"+1));
  strcpy(buffer,"   cat");
  printf("%s\n",trim(buffer));
}
{% endhighlight %}
Here, we change the content of the pointer. Question: why we need **strlen(phrase)+1**? We can just change the pointer in the **trim** function.
{% highlight objc %}
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
char* trim(char** phrase)
{
  char* old=*phrase;
  while(*old==' '){
    old++;
  }
  return old;
}
int main(){
  char* buffer=(char*)malloc(strlen("  cat")+1);
  strcpy(buffer,"  cat");
  printf("%s\n",trim(&buffer));
}
{% endhighlight %}

## Using Pointer Notation
Instead of using the bracket notation when declaring an array parameter, we can use pointer notation as follows:
{% highlight objc %}
void displayArray(int* arr, int size){
	for(int i=0;i<size;i++){
		printf("%d\n",arr[i])
}
}
{% endhighlight %}
To pass the **matrix**, use either:
	void display2DArray(int arr[][5], int rows)
	void display2DArray(int (*arr)[5], int rows)
In the first version, the expression **arr[]** is an implicit declaration of a pointer to an array. In the second version, the expression **(*arr)** is an explicit declaration of the pointer.

## Dynamically Allocating a Two-Dimensional Array

- Whether the array elements need to be contiguous
- Whether the array is jagged

The following code shows on way of allocating a 2D array where the allocated memory is not guranteed to be contiguous.
{% highlight objc %}
int rows=2;
int columns=5;
int **matrix=(int **)malloc(row*sizeof(int *))
for(int i=0;i<rows;i++){
matrix[i]=(int *)malloc(columns*sizeof(int));
}
{% endhighlight %}
Since separate **malloc** calls were used, the allocated memory is not guranteed to be contiguous.The actual allocation depends on the heap manager and the heap's state. It may well be contiguous.

We present two approaches for allocating contiguous memory for 2D array.
{% highlight objc %}
int rows=2;
int columns=5;
int **matrix=(int **)malloc(rows*sizeof(int*));
matrix[0]=(int*)malloc(rows*columns*sizeof(int));
for(int i=1;i<rows;i++)
	marix[i]=matrix[0]+i*columns;
{% endhighlight %}
In the second technique, all of the memory for the array is allocated at one time:
	int *matrix=(int *)malloc(rows*columns*sizeof(int));
Question: Why allocating 2D array in contiguous memory is much better?

