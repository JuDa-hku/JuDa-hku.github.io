---
layout: post
title: Pointers and Functions
categories:
- programming
tags:
- C
---
* Passing Data Using a Pointer
* Returning a Pointer
* Pointer to local data
* Passing Null Pointers
* Passing a Pointer to a Pointer
* Function Pointers
* Summary

---

There are two areas where pointers become useful, when working with functions.

- We pass pointer to a function. This allows function modify data referenced by the pointer and to pass blocks of information more efficiently.
- Function pointers provide additional capability to control the execution flow of a program as we will see.

## Passing Data Using a Pointer
One of the primary reasons for passing data using a pointer is to allow the function to modify the data.
{% highlight objc %}
void swapWithPointers(int* pnum1, int* pnum2){
int tmp;
tmp=*pnum1
*pnum1=*pnum2
*pnum2=tmp
}
int main(){
int n1=5;
int n2=10;
swapWithPointers(&n1,&n2);
return 0;
}
{% endhighlight %}
If we do not pass value through point, we are not modifying the original arguments when we modify the parameters.

## Returning a Pointer
**Returning a pointer** is easy to do, we simply declare the return type to be a pointer to the appropriate data type.
{% highlight objc %}
int* allocateArrary(int size, int value){
int* arr=(int*)malloc(size * sizeof(int));
for(int i=0;i<size;i++){
arr[i]=value;
}
return arr;
}
int* vector=allocateArray(5,45);
for(int i=0;i<5;i+=){
printf("%d\n",vector[i]);
}
{% endhighlight %}
While the **arr** went away when the function terminated, the memory referenced by the pointer does not go away. Use **free(vector)** in the end. Several potential problems can occur including:

- Returning an uninitialized pointer
- Returning a pointer to an invalid address
- Returning a pointer to a local variable
- Returning a pointer but failing to free it

## Pointers to local Data (common mistake)
Returning a pointer to local data is an easy mistake to make if you don't understand how the program stack works.
{% highlight objc %}
int* allocateArray(int size, int value){
	int arr[size];
	for(int i=0; i<size; i++){
		arr[i]=value;
	}
}
int* vector=allocateArray(5,45);
for(int i=0; i<5;i++){
	printf("%d\n",vecotr[i])
}
{% endhighlight %}
Unfortunately, the address of the arrary returned **arr** is not longer valid once the function returns because the function's statck frame is popped off the stack.

## Passing Null Pointers
In the **allocateArray** version, apointer to an array is passed along with its size and a value that it will use to initialize each element of the array.
{% highlight objc %}
int *allocateArray(int *arr, int size, int value){
	if(arr !=NULL){
		for(int i=0;i<size;i++){
			arr[i]=value;
	}
		}
}
int *vector=(int*)malloc(5*sizeof(int));
allocateArray(vector,5,45)
{% endhighlight %}

## Passing a Pointer to a Pointer
When a pointer is passed to a function, it is passed by value. If we want to modify the original pointer and not the copy of the pointer, we need to pass it as a pointer to a pointer.
{% highlight objc %}
void allocateArray(int **arr, int size, int value){
	*arr=(int*)malloc(size * sizeof(int))
	if(*arr!=NULL){
		for(int i=0;i<size;i++){
			*(*arr+i)=value;
		}
	}
}
int *vector=NULL;
allocateArray(&vector,5,45);
{% endhighlight objc %}
Here, we want to change pointer **vector** so we use a pointer to it.
The following version of the function illustrate why passing a simple pointer will not work:
{% highlight objc %}
void allocateArray(int *arr, int size, int value){
	arr=(int*)malloc(size * sizeof(int))
	if(arr!=NULL){
		for(int i=0;i<size;i++){
			arr[i]=value;
		}
	}
}
int *vector=NULL;
allocateArray(&vector,5,45);
printf("%p\n",vector)
{% endhighlight objc %}
because we cannot change **vector**, it will still be the NULL.

## Function Pointers
We declare a pointer to a function that is passed void and returns void:
	void (* foo) ();
Other examples of function pointer declarations are illustrated below:
{% highlight objc %}
int (*f1)(double); //passed a double and returns an int
int (*f2)(charr*); //passed a pointer to char and returns void
double* (*f3)(int,int);
{% endhighlight %}
The example of how we use the function pointer.
{% highlight objc %}
int (*fptr)(int);
int square(int num){
	return num*num;
}
int n=5;
fptr=square;
printf("%d squared is %d\n",n,fptr(n));
{% endhighlight %}
It is convenient to declare a type definition for function pointers. This is illustrated below for the previous function pointer.
{% highlight objc %}
typedef int (*funcptr)(int);
...
funcptr fptr2;
fptr2=square;
printf("%d squared is %d\n",n,fptr(n));
{% endhighlight %}
Passing a function pointer is easy. Simply use a function pointer declaration as a parameter of a function.
{% highlight objc %}
int add(int num1, int num2){
	return num1+num2;
}
int substract(int num1, int num2){
	return num1-num2;
}
typedef int (*fptrOperation)(int,int);
int compute(fptrOperation operation, int num1, int num2){
	return operation(num1,num2);
}
printf("%d\n",compute(add,3,4));
printf("%d\n",compute(substract,4,3));
{% endhighlight %}
Of course, we can return a function pointer first through the **select** below and pass it to the compute function.
{% highlight objc %}
fptrOperation select(char opcode){
	switch(opcode){
		case '+': return add;
		case '-': return substract;
	}
}
{% endhighlight %}
Arrays of function pointers can be used to select the function to evaluate on the basis of some criteria.
{% highlight objc %}
typedef int (*operation)(int,int);
operation operations[128]={NULL};
void initializeOperationsArray(){
	operations['+']=add;
	operations['-']=subtract;
}
int evaluateArray(char opcode, int num1, int num2){
	fptrOperation operation;
	operation=operations[opcode];
	return operation(num1,num2);
}
initializeOperationsArray();
printf("%d\n",evaluateArray('+',5,6));
{% endhighlight %}

## Summary
Understatnding the program stack and heap structures contributes to a more detailed and thorough understanding of how a program works and how pointers behave. Returning a pointer to a local variable is bad because the memory allocated to the local variable will be overwritten by subsequent function calls. Passing a pointer to constant data is efficient and prevents the function from modifying the data passed. Passing a pointer to a pointer allows the argument pointer to be reasigned to a different location in memory. Function pointers are useful for controlling the execution sequence.
