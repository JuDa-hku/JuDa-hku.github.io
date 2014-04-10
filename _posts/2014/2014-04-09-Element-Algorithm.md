---
layout: post
title: Algorithm Element (2)
categories:
- Programming
tags:
- C
- Algorithm

---
* Shellsort
* Heapsort

---

## Shellsort
Shellsort, named after its inventor,Donald Shell, was one of the first algorithms to break the quadratic time barrier. Shellsort uses a sequence, h_1,h_2,...,h_t called the *increment sequence*. Any increment sequence will do as long as h_1=1, but some are better than others. All elements spaced h_k apart are sorted. A popular (but poor) choice for increment sequence is to use the sequence suggested by Shell: h_t=int(N/2),h_k=int(h_(k+1)/2).
{% highlight objc %}
#include<stdio.h>
int a[100],n;
void Initialize(void)
{
  printf("input the number of element: ");
  scanf("%d",&n);
  printf("Input the whole with %d numbers:", n);
  for(int i=0;i<n;i++)
    {
      scanf("%d",&a[i]);
    }
}

void ShellSort(void)
{
  int tmp,increment,j;
  for(increment=n/2;increment>=1;increment/=2)
    for(int i=increment;i<n;i++)
      {
	tmp=a[i];
	for(j=i;j>=increment;j-=increment)
	  if(tmp<a[j-increment])
	    a[j]=a[j-increment];
	  else
	    break;
	a[j]=tmp;
	/*	if(a[i-increment]>a[i])
	  {
	    a[i]=a[i-increment];
	    a[i-increment]=tmp;
	    } Think about why this does not work*/
      }
}
{% endhighlight %}

## Heapsort
Priority queues can be used to sort in O(NlogN). The algorithm based on this idea is known as *Heapsort*. In practice, it is slower than a version of Shellsort that uses Sedgewick's increment sequence. In our implementation, we will use a (max)heap, but avoid the actual ADT for the purpose of speed. Everything is done in an array. The first step builds the heap in linear time. We then perform N-1 DeleteMaxes by swapping the last element in the heap with the first, decrementing the heap size, and percolating down.
{% highlight objc %}
#include<stdio.h>
#define LeftChild(i) (2*i+1)
//Use array to build the heap, for position i the leftchild it is in position 2i+1

int a[100],n;
void Initialize(void)
{
  printf("input the number of element: ");
  scanf("%d",&n);
  printf("Input the whole with %d numbers:", n);
  for(int i=0;i<n;i++)
    {
      scanf("%d",&a[i]);
    }
}

void PerDown(int a[],int i , int N)
{
  int Child;
  int tmp;
  for(tmp=a[i];i<N;i=Child)
    {
      Child=LeftChild(i);
      if(Child!=N-1&&a[Child+1]>a[Child])
	Child++;
      if(a[Child]>tmp)
	  a[i]=a[Child];
      else 
	break;
    }
  a[i]=tmp;
}
/* Think about how we build the heap recrusively by using
   the PerDown function*/

void Swap(int* a, int *b)
{
  int * tmp;
  tmp=b;
  b=a;
  a=tmp;
}

void HeapSort(int a[], int N)
{
  int i;
  for(i=N/2;i>=0;i--)
    PerDown(a,i,N);
	/* build the heap */
  for(i=N-1;i>0;i--)
    {
      Swap(&a[0],&a[i]);
	  /* deleteMax */
      PerDown(a,0,i);
    }
}
{% endhighlight %}




