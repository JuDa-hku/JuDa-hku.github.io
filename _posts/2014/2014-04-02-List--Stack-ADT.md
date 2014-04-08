---
layout: post
title: List and Stack data abstract
categories:
- Programming
tags:
- C
- ADT

---
* List 
* Stack

---

## List
The List.h can be written as, here you can define your own **ElementType**.
{% highlight objc %}
#ifndef _LIST_H //define the tag in case of redefine the variable
#define _LIST_H
typedef int ElementType;
struct  Node;
typedef struct Node *PtrToNode;
typedef PtrToNode List;
typedef PtrToNode Position;

List MakeEmpty(List L);
int IsEmpty(List L);
int IsLast(Position P, List L);
Position Find(ElementType X, List L);
void Delete(ElementType X, List L);
Position FindPrevious(ElementType X, List L);
void Insert(ElementType X, List L, Position P);
void DeleteList(List L);
Position Header(List L);
Position Advance(Position P);
ElementType Retrieve(Position P);

#endif /*_LIST_H*/

{% endhighlight %}
The implementation of the list abstract structure can be written as:
{% highlight objc %}
#include<stdio.h>
#include<stdlib.h>
#include"list.h"

struct Node{
  ElementType Element;
  Position Next;
};

int IsEmpty(List L){
  return L->Next==NULL;
}

int IsLast(Position P, List L)
{
  return P->Next==NULL;
}

Position Find(ElementType X, List L){
  Position P;
  P=L->Next;
  while(P!=NULL&&P->Element!=X)
    P=P->Next;
  return P;
}

Position FindPrevious(ElementType X, List L){
  Position P;
  P=L;
  while(P->Next!=NULL&&P->Next->Element!=X)
    P=P->Next;
  return P;
}

void Delete(ElementType X, List L){
  Position P, TmpCell;
  P=FindPrevious(X,L);
  if(!IsLast(P,L)){
    TmpCell=P->Next;
    P->Next=TmpCell->Next;
    free(TmpCell);
  }
}
   

void Insert(ElementType X, List L, Position P){
  Position TmpCell;
  TmpCell=(Position)malloc(sizeof(struct  Node));
  if(TmpCell==NULL)
    printf("Out of space!!");
  TmpCell->Element=X;
  TmpCell->Next=P->Next;
  P->Next=TmpCell;
}

void DeleteList(List L){
  Position P, Tmp;
  P=L->Next;
  L->Next=NULL;
  while(P!=NULL)
    {
      Tmp=P->Next;
      free(P);
      P=Tmp;
    }
}

int main()
{
  List L;
  PtrToNode p;
  p=(PtrToNode)malloc(sizeof(PtrToNode));
  p->Element=3;
  L->Next=p;
int  a=sizeof(PtrToNode);
 Insert(4,L,p);
 Insert(5,L,p); // Insert two integer after the p
 printf("%d\n%d\n",a,p->Next->Element);
 Delete(5,L);  //to test the Delete function
 printf("%d\n",p->Next->Element);
 printf("%d\n",int(L==NULL));
 DeleteList(L);
}
{% endhighlight %}

## Stack
The Stack.h can be written as below, here you can define your own **ElementType**.
{% highlight objc %}
#ifndef _STACK_H
#define _STACK_H
#define ElementType const char*
struct Node;
typedef struct Node *PtrToNode;
typedef PtrToNode Stack;

int IsEmpty(Stack S);
Stack CreateStack(void);
void DisposeStack(Stack S);
void MakeEmpty(Stack S);
void Push(ElementType X, Stack S);
ElementType Top(Stack S);
void Pop(Stack S);

#endif /*_STACK_H*/
{% endhighlight %}
The implementation of the structure are showed below:
{% highlight objc %}
#include<stdlib.h>
#include<stdio.h>
#include"Stack.h"

struct Node
{
  ElementType Element;
  PtrToNode Next;
};

int IsEmpty(Stack S){
  return(S->Next==NULL);
}

Stack CreatStack(){
  PtrToNode S=(PtrToNode)(malloc(sizeof(Stack)));
  if(S==NULL)
    printf("Fail to allocate memory");
  MakeEmpty(S);
  return S;
}

void MakeEmpty(Stack S){
  if(S==NULL)
    printf("Use CreatStack First");
  else 
    while(!IsEmpty(S))
      Pop(S);
}

void Push(ElementType X, Stack S){
  PtrToNode TmpCell;
  TmpCell=(PtrToNode)malloc(sizeof(PtrToNode));
  if(TmpCell==NULL)
    printf("Fail to allocate memory");
  else{
    TmpCell->Element=X;
    TmpCell->Next=S->Next;
    S->Next=TmpCell;
  }   
}

ElementType Top(Stack S){
  if(!IsEmpty(S)){
    printf("%s\n",S->Next->Element);
    return S->Next->Element;
   }
  printf("Empty stack");
}

void Pop(Stack S){
  PtrToNode FirstCell;
  if(IsEmpty(S))
    printf("Empty stack");
  else{
    FirstCell=S->Next;
    S->Next=S->Next->Next;
    free(FirstCell);
  }
}

int main(){
  PtrToNode S;
const char* p;
  p="dgong";
  S=CreatStack();
  Push(p,S);
  Push(p,S);
  p=Top(S);
  MakeEmpty(S);
}
{% endhighlight %}


