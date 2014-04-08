---
layout: post
title: Algorithm Element (1)
categories:
- Programming
tags:
- C
- Algorithm

---
* InsertSort
* BucketSort
* Bubble Sort
* Quick Sort
* Floyd Algorithm
* Dijkstra Algorithm

---

## InsertSort
Use the mathmatic inducation method to analysis the algorithm. To insert the value to the sorted sequence, we need to find the first number that is smaller or bigger than the given one. Then move the part of the array from the found number to the end back one. At last, we insert the number into this position.
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

void InsertSort(void)
{
  int tmp;
  for (int i = 1; i < n; ++i)
    {
      tmp=a[i];
      while(a[i-1]>tmp&&i>=0)
	{
	  a[i]=a[i-1];
	  i--;
	}
      a[i]=tmp;
    }
}

void output(void)
{
  for (int i = 0; i < n; ++i)
    {
      printf("%d ",a[i]);
    }
}

int main()
{
  Initialize();
  InsertSort();
  output();
}
{% endhighlight %}

## BucketSort
{% highlight objc %}
#include<stdio.h>
#define MAX 1001
int main()
{
  int a[MAX],number,m;
  for(int i=0;i<MAX;i++)
    a[i]=0;
  scanf("%d",&number);
  for(int j=0;j<number;j++)
    {
      scanf("%d",&m);
      a[m]++;
    }
  for(int i=0;i<MAX;i++)
    for(int j=0;j<a[i];j++)
      printf("%d ",i);
}
{% endhighlight %}
The BucketSort can sort the integer number within 0-MAX very fast. It just count numbers of times one integer show up in the sequence. When we print the array from 0-MAX, we finish the sorting.

## Bubble Sort.
We compare the number with the next one in the sequence, in this way we gurantee that the biggest will in the last position after one iteration.
{% highlight objc %}
#include<stdio.h>
struct student
{
  char name[21];
  int score;
};
int main()
{
  struct student a[100];
  int number;
  scanf("%d",&number);
  for(int i=0;i<number;i++)
    scanf("%s %d",a[i].name,&a[i].score);
  for(int i=0;i<number-1;i++)
    for(int j=i;j<number-1;j++)
      if(a[j].score<a[j+1].score)
	{
	  int tmp;
	  tmp=a[j+1].score;
	  a[j+1].score=a[j].score;
	  a[j].score=tmp;
	}
  for (int i = 0; i < number; ++i)
    {
      printf("%s %d ",a[i].name,a[i].score);
        }
}
{% endhighlight %}


## QuickSort
In one iteration, we regard the first number as the CRETERION, numbers that smaller than it will be in the left and bigger number will be in the right. We search the sequence from the left and right at the same time. If we meet with a number that is larger than the CRETERION in the right sigh and a number smaller, we change these two numbers and when the search from right and left come across in the middle of the sequence, we change the number with the first number(the CRETERION). Pay attention, we should search from the end first, find the larger one and stop, then search from the head, find the smaller one then swap these two numbers. Consider why!!
{% highlight objc %}
#include<stdio.h>
int a[100],n;
void quick(int left, int right)
{
  int tmp,i,j,t;
  if(left>right)
    return ;

 tmp=a[left];
 i=left;
 j=right;
 while(i!=j)
   {
     while(a[j]>=tmp&&j>i)
       j--;
     while(a[i]<=tmp&&i<j)
       i++;
     if(i<j)
       {
	 t=a[i];
	 a[i]=a[j];
	 a[j]=t;
       }
   }
 a[left]=a[i];
 a[i]=tmp;
 quick(left,i-1);
 quick(i+1,right);
}
 
int main()
{
  scanf("%d",&n);
  for(int i=0;i<n;i++)
    {
      scanf("%d",&a[i]);
	}
  quick(0,n-1);
 for(int i=0;i<n;i++)
    {
      printf("%d ",a[i]);
	}
}
{% endhighlight %}

## Floyd Algorithm
Find the shortest way from one point to another point. We update the way step by step. We consider if there is any nearer ways from i to j through 1 first then if there is any nearer ways from i to j through 2. Iterate step by step.
{% highlight objc %}
#include<stdio.h>
int main()
{
  int e[10][10],point,edge,i,j,p1,p2,len,k,m;
  int inf=99999;
  scanf("%d %d",&point,&edge);
  for(i=1;i<10;i++)
    for(j=1;j<10;j++)
      if(i==j)
	e[i][j]=0;
      else
	e[i][j]=inf;
  for(i=1;i<=edge;i++)
    {
      scanf("%d %d %d",&p1,&p2,&len);
      e[p1][p2]=len;
    }
  for(k=1;k<=point;k++)
    for(i=1;i<=point;i++)
      for(j=1;j<=point;j++)
	if(e[i][k]+e[k][j]<e[i][j])
	  e[i][j]=e[i][k]+e[k][j];
  for(i=1;i<=point;i++)
    for(j=1;j<=point;j++)
      if(e[i][j]!=inf)
	printf("%d %d %d\n",i,j,e[i][j]);
}
{% endhighlight %}


## Dijkstra algorithm
We find the shortest way of point 1 to other points. First, we find the shortest edge from 1 to another point i. And mark it as the shortest with book[i]=1, through this i we can find other ways from 1 to other points, if the length of these points is shorter that 1 to j directly. We find a shorter way. Then we use this method again, we pick up the point that is nearest to 1 denote it as i1 and pay attention the first picked point is excluded with book[i]=1. The shortest 1 to i1 can not be shorter because we have compared with the distance from 1 to i then i1. Through **points-1** times iteration, we can find all shortest way from 1 to all other points.
{% highlight objc %}
#include<stdio.h>
int main()
{
  int e[10][10],s[10],book[10],edge,point,i,j,p1,p2,start,u;
  int inf=99999;
  scanf("%d%d",&point,&edge);
  for(i=1;i<10;i++)
    for(j=1;j<10;j++)
      {
	if(i!=j)
	  e[i][j]=inf;
	else
	  e[i][i]=0;
      }
  for(i=1;i<=edge;i++)
    {
      scanf("%d%d",&p1,&p2);
      scanf("%d",&e[p1][p2]);
    }
  printf("input the point from which you start search the shortest way ");
    scanf("%d",&start);
  for(i=1;i<=point;i++)
    {
    book[i]=0;
    s[i]=e[start][i];
    }
  book[start]=1;

  for(i=1;i<=point;i++)
    {
      int min=inf;
      for(j=1;j<=point;j++)
	{
	  if(book[j]==0&&s[j]<min)
	    {
	      min=s[j];
	      u=j;
	    }
	}
      book[u]=1;
      for(j=1;j<=point;j++)
	{
	  if(e[u][j]<inf)
	    {
	      if(s[j]>s[u]+e[u][j])
		s[j]=s[u]+e[u][j];
	    }
	}
    }
  for(i=1;i<=point;i++)
    printf("%d ",s[i]);
  return 0;
}

/*
6 9
1 2 1
1 3 12
2 3 9
2 4 3
3 5 5
4 3 4
4 5 13
4 6 15
5 6 4
*/
{% endhighlight %}

