---
layout: post
title: Queue and Bianry Search Trees
categories:
- Programming
tags:
- C
- ADT

---
* Queue
* Binary Search Trees

---

## Queue
Queue can be regared as First-in-First-out(FIFO) list. Here we use array to store the all queue, it is bascially the same as pointer but the capacity of the array has to be given and cannot be malloced dynamically. The Queue.h is wriiten below:
{% highlight objc %}
//Array implementation of queue
#ifndef _Queue_h
#define _Queue_h
#define ElementType int
struct QueueRecord;
typedef struct QueueRecord *Queue;

int IsEmpty(Queue Q);
int IsFull(Queue Q);
Queue CreateQueue(int MaxElements);
void DisposeQueue(Queue Q);
void MakeEmpty(Queue Q);
void Enqueue(ElementType X,Queue Q);
ElementType Front(Queue Q);
void Dequeue(Queue Q);
ElementType FrontAndDequeue(Queue Q);

#endif // _Queue_h
{% endhighlight %}

The implementation of Queue structure is :
{% highlight objc %}
#include<stdlib.h>
#include<stdio.h>
#include"Queue.h"
struct  QueueRecord
{
  int Capacity;
  int Front;
  int Rear;
  int Size;
  ElementType *Array;
};

int IsEmpty(Queue Q)
{
  return Q->Size==0;
}

void MakeEmpty(Queue Q)
{
  Q->Size=0;
  Q->Front=1;
  Q->Rear=0;
}

int IsFull(Queue Q)
{
  return Q->Size==Q->Capacity;
}


static int Succ(int Value, Queue Q)
{
  if(++Value==Q->Capacity)
    Value=0;
  return Value;
}

void Enqueue(ElementType X, Queue Q)
{
  if(IsFull(Q))
    printf ("Full queue \n");
  else
    {
      Q->Size++;
      Q->Rear=Succ(Q->Rear,Q);
      Q->Array[Q->Rear]=X;
    }
}

ElementType Front(Queue Q)
{
  return Q->Array[Q->Front];
}

void Dequeue(Queue Q)
{
  if(IsEmpty(Q))
    printf ("No element in the Queue \n");
  else
    {
      Q->Size--;
      Q->Front++;
    }
}

ElementType FrontAndDequeue(Queue Q)
{
  if(IsEmpty(Q))
    printf ("No element in the Queue \n");
  else
 {
      Q->Size--;
      Q->Front++;
      return Q->Array[Q->Front];
    }
}

Queue CreateQueue(int MaxElements)
{
  Queue Q;
  Q=(Queue)malloc(sizeof(Queue));
  if(Q==NULL)
    printf ("out of memory\n");
  Q->Array=(ElementType*)malloc(sizeof(ElementType)*MaxElements);
  if(Q->Array==NULL)
    printf ("Out of memory \n");
  Q->Capacity=MaxElements;
  MakeEmpty(Q);
  return Q;
}

int main()
{
  Queue Q;
  int a,b,c,d;nn
  Q=CreateQueue(10);
  a=IsEmpty(Q);
  Dequeue(Q);
  Enqueue(3,Q);
  b=IsEmpty(Q);
  Enqueue(4,Q);
  c=Front(Q);
  FrontAndDequeue(Q);
  d=Front(Q);
  Dequeue(Q);
  printf ("%d\n%d\n%d\n%d\n",a,b,c,d);
}
{% endhighlight %}

## Binary Search Trees
A simplified binary search trees is given below. The Tree.h file
{% highlight objc %}
#ifndef _Tree_H
#define _Tree_H
#define ElementType int

struct TreeNode;
typedef struct TreeNode *Position;
typedef struct TreeNode *SearchTree;

SearchTree MakeEmpty(SearchTree T);
Position Find(ElementType X, SearchTree T);
Position FindMin(SearchTree T);
Position FindMax(SearchTree T);
SearchTree Insert(ElementType X, SearchTree T);
SearchTree Delete(ElementType X, SearchTree T);
ElementType Retrieve(Position P);

#endif // _Tree_H
{% endhighlight %}
The implementation of the structure is given in tree.c file:
{% highlight objc %}
#include<stdlib.h>
#include<stdio.h>
#include"Tree.h"

struct TreeNode
{
  ElementType Element;
  SearchTree Right;
  SearchTree Left;
};

SearchTree MakeEmpty(SearchTree T){
  if(T!=NULL){
    MakeEmpty(T->Right);
    MakeEmpty(T->Left);
    free(T);
  }
  return NULL;
}

Position Find(ElementType X, SearchTree T){
  if(T==NULL)
    return NULL;
  if(X<T->Element)
    return Find(X,T->Left);
  else if(X>T->Element)
    return Find(X,T->Right);
  else return T;
}

Position FindMin(SearchTree T){
  if(T==NULL){
    printf("Empty tree \n");
    return NULL;
  }
  else if(T->Left==NULL)
    return T;
  else return FindMin(T->Left);
}

Position FindMax(SearchTree T){
  if(T==NULL)
    return NULL;
  else if(T->Right==NULL)
    return T;
  else return FindMax(T->Right);
}

SearchTree Insert(ElementType X,SearchTree T){
  if(T==NULL){
    T=(SearchTree)malloc(sizeof(TreeNode));
    if(T==NULL)
      printf ("Not enough space\n");
    else{
    T->Element=X;
    T->Left=T->Right=NULL;
    }
  }
  else if(X<T->Element){
       T->Left=Insert(X,T->Left);
  }
  else if(X>T->Element){
    T->Right=Insert(X,T->Right);
  }
  return T;
}

SearchTree Delete(ElementType X, SearchTree T) // check how to delete, not easy
{
  Position TmpCell;
  if(T==NULL)
    printf("Element not found");
  else if(X<T->Element)
    T->Left=Delete(X,T->Left);
  else if(X>T->Element)
    T->Right=Delete(X,T->Right);
  else if(T->Left&&T->Right) // Two children
    {
      TmpCell=FindMin(T->Right);
      T->Element=TmpCell->Element;
      T->Right=Delete(T->Element,T->Right);
    }
  else /* one or zero children */
    {
      TmpCell=T;
      if(T->Left==NULL)
	T=T->Right;
      else if(T->Right==NULL)
	T=T->Left;
      free(TmpCell);
    }
  return T;
}

int main(){
  SearchTree T;
  printf("%p\n",T);
  SearchTree S=(SearchTree)malloc(sizeof(TreeNode));
  printf("%p\n",S);
  FindMin(T);
  printf("%d\n",int(T==NULL));
  T=Insert(-2,T);
  T=Insert(-3,T);
  printf ("%d\n",FindMin(T)->Element);
  printf("%p\n",T);
  printf("%p\n",FindMin(T));
  S=FindMin(T);
  printf("%p\n",S);
  Delete(-3,T);
  printf ("%d\n",FindMin(T)->Element);
}
{% endhighlight %}
The hardest part of the program is the **Delete** function, in fact, the code do this recrusively and replace the data of this node with the smallest data of the right subtree for deleting the node with two children.








