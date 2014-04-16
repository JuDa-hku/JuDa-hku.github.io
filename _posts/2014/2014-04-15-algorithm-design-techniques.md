---
layout: post
title: Algorithm Design Techniques through Sub-Summation example
categories:
- Programming
tags:
- C
- Algorithm

---
* Three Algorithm
* Principles

---

## Simple example
The input is a vector x of n floating-point numbers; the output is the maximum sum found in any contiguous subvector of the input. Toy code generates the data is below
{% highlight objc %}
#include<stdio.h>
#include<stdlib.h>
#define N 100000
int main()
{
  FILE *pFile;
  pFile=fopen("data","w");
  if(pFile!=NULL)
    {
      for(int i=0;i<N;i++)
	{
	  int j=rand()%100;
	  if(j>50)
	  fprintf(pFile,"%d ",-rand()%100);
	  else
	  fprintf(pFile,"%d ",rand()%100);
	}
      fclose(pFile);
    }
}
{% endhighlight %}

##A O(n^3) algorithm
This code is short, straightforward and easy to understand. Unfortunately, it is also slow.
{% highlight objc %}
#include<stdio.h>
#include<time.h>
#define N 100000
int main()
{
  clock_t t=clock();
  int a[N],i,j;
  int result,summation;
  result=summation=0;
  FILE * pFile;
  pFile=fopen("data","r");
  for(i=0;i<N;i++)
    fscanf(pFile,"%d",&a[i]);
  for(i=0;i<N;i++)
    {
     for(j=i;j<N;j++)
      {
	summation=0;
	for(int k=i;k<j;k++)
	  summation+=a[k];
	if(summation>result)
	  result=summation;
      }
    }
  t=clock()-t;
  float time=float(t)/CLOCKS_PER_SEC;
  printf("%f %d",time,result);
}
{% endhighlight %}

## A O(n^2) Algorithm
The algorithm inspect all possible pairs of starting and ending values of subvectors and evaluate the sum of the numbers in the subvect, and there are O(n^2) subvectors.
{% highlight objc %}
#include<stdio.h>
#include<time.h>
#define N 100000
int main()
{
  clock_t t=clock();
  int a[N],i,j;
  int result,summation;
  result=summation=0;
  FILE * pFile;
  pFile=fopen("data","r");
  for(i=0;i<N;i++)
    fscanf(pFile,"%d",&a[i]);
  for(i=0;i<N;i++)
    {
    summation=0;
    for(j=i;j<N;j++)
      {
	summation+=a[j];
	if(summation>result)
	  result=summation;
      }
    }
  t=clock()-t;
  float time=float(t)/CLOCKS_PER_SEC;
  printf("%f %d",time,result);
}
{% endhighlight %}

##A Divide and Conquer Algorithm
To solve a problem of size n, recursively solve two subproblems of size approximately n/2, and combine their  solutions to yield a solution to the complete problem.
{% highlight objc %}
#include<stdio.h>
#include<time.h>
#include<math.h>
#define N 100000
int a[N];
float maxsum3(int l, int u)
{
  if(l>u)
    return 0;
  if(l==u)
    {
      if(a[l]>0)
	return a[l];
      else
	return 0;
    }
  int m=(l+u)/2;
  int lmax,sum,i;
  lmax=sum=0;
  for(i=m;i>=l;i--)
    {
      sum+=a[i];
      lmax=fmax(lmax,sum);
    }
  int rmax;
  rmax=sum=0;
  for(i=m+1;i<=u;i++)
    {
      sum+=a[i];
      rmax=fmax(rmax,sum);
    }
  return fmax(fmax(lmax+rmax,maxsum3(l,m)),maxsum3(m+1,u));
}

int main()
{
  clock_t t=clock();
  int i,j;
  int result,summation,maxsofar;
  result=summation=maxsofar=0;
  FILE * pFile;
  pFile=fopen("data","r");
  for(i=0;i<N;i++)
    fscanf(pFile,"%d",&a[i]);
  result=maxsum3(0,N-1);
  t=clock()-t;
  float time=float(t)/CLOCKS_PER_SEC;
  printf("%f %d",time,result);
}
{% endhighlight %}
The code is subtle and esay to get wrong, but it solves the problem in O(nlogn) time.

## A Scanning Algorithm
Suppose that we've solved the problem for x[0..i-1]; how can we extend that to include x[i]? The maximum-sum in the first i is either in the first i-1 elements or it ends in position i.
{% highlight objc %}
#include<stdio.h>
#include<time.h>
#define N 100000
int main()
{
  clock_t t=clock();
  int a[N],i,j;
  int result,summation,maxsofar;
  result=summation=maxsofar=0;
  FILE * pFile;
  pFile=fopen("data","r");
  for(i=0;i<N;i++)
    fscanf(pFile,"%d",&a[i]);
  for(i=0;i<N;i++)
    {
    summation+=a[i];
    if(summation>maxsofar)
      maxsofar=summation;
    summation=int(summation>0)*summation;
    }
  result=maxsofar;
  t=clock()-t;
  float time=float(t)/CLOCKS_PER_SEC;
  printf("%f %d",time,result);
}
{% endhighlight %}
The key to understanding this program is **maxendinghere**. Before the first assignment statement in the loop, **maxendinghere** contains the value of the maximum subvector ending in position i-1; the assignment statement modifies it to contain the value of the maximum subvecotr ending in position i.

## Principles

- Save state to avoid recomputation. By using space to store results, we avoid using time to recompute them.
- Preprocess information into data structures.
- Divid-and-conquer algorithms.
- Scanning algorithms. Problems on arrays can often be sloved by asking "how can I extend a solution for x[0..i-1] to x[0..i]".
- Lower bounds. Algorithm designers sleep peacefully only when they know their algorithms are the best possible; for this assurance they must prove a matching lower bound. More complex lower bounds can be quite difficult.x
