---
layout: post
title: Priority Queues(Heaps)
categories:
- Programming
tags:
- C
- ADT

---
* Binary Heap
* Leftist Heaps
* Binomial Queues 

---

Although jobs sent to a printer are generally placed on a queue, this might not be the best thing. For instance, one job might be particularly important, so it might be desirable to allow that job to be run as soon as the printer is available. Conversely, if, when the printer becomes available, there are serveral 1-page jobs and one 100-page job, it might be reasonable to make the long job go last, even if it is not the last job submitted.

## Binary Heap
The implementation we will use is known as a *binary heap*. Its use is so common for priority queue implementations that when the word heap is used without a qualifier, it is generally assumed to be referring to this. A heap is a binary tree that is **completely filled**, with the possible exception of the bottom level, which is filled from left to right. A complete binary tree of height h has between 2^h and 2^(h+1)-1 nodes. The property that allows operations to be performed quickly is the *heap order* property. For every node X, the key in the parent of X is smaller than the key in X.
{% hightlight objc %}
#ifndef _BinHeap_H

struct HeapStruct;
typedef struct HEapStruct *PriorityQueue;

PriorityQueue Initialize(int MaxElements);
void Destory(PriorityQueue H);
void MakeEmpty(PriorityQueue H);
void Insert(ElementType X, PriorityQueue H);
ElementType DeleteMin(PriorityQueue H);
ElementType FindMin(PriorityQueue H);
int IsEmpty(PriorityQueue H);
int IsFull(PriorityQueue H);

#endif
{% endhighlight objc}
The implementation file for *initializing*
{% highlight objc %}
struct HeapStruct
{
  int Capacity;
  int Size;
  ElementType *Elements;
};

PriorityQueue Initialize(int MaxElements)
{
  PriorityQueue H;
  if(MaxElements<MinPQSize)
    Error("Priority Queue is too small");
  H=malloc(sizeof(struct HeapStruct));
  if(H==NULL)
    FatalError("Out of space");

  H->Elements=malloc((MaxElements+1)*sizeof(ElementType));
  if(H->Elements==NULL)
    FatalError("Out of space");
  H->Capacity=MaxElements;
  H->Size=0;
  H->Elements[0]=MinData;

  return H;
}
{% endhighlight %}
To insert an element X into the heap, we create a hole in the next available location. If X can be there, it is OK. Otherwise we slide the element that is in the hole's parent node into the hole, thus bubbling the hole up toward the root.
{% highlight objc %}
void Insert(ElementType X, PriorityQueue H)
{
  int i;
  if(IsFull(H))
    {
      Error("Priority queue is full");
      return ;
    }
  /* here we use array to store data i/2 is the parent node of the new estabilished circle */
  for(i=++H->Size;H->Elements[i/2]>X;i/=2)
    H->Elements[i]=H->Elements[i/2];
  H->Elements[i]=X;
}
{% endhighlight %}
*DeleteMins* are handled in a similar manner as insertions. Finding the minimum is easy, the hard part is removing it. A frequent implementation error in heaps occurs when there are an even number of elements in the heap, and the one node that has only one child is encountered.
{% highlight objc %}
ElementType DeleteMin(PriorityQueue H)
{
  int i, Child;
  ElementType MinElement, LastElement;
  if(IsEmpty(H))
    {
      Error("Priority queue is empty");
      return H->Elements[0];
    }
  MinElement=H->Elements[1];
  LastElement=H->Elements[H->Size--];
  
  for(i=1;i*2<=H->Size;i=Child)
    {
      Child=i*2;
      if(Child!=H->Size&&H->Elements[Child+1]<H->Elements[Child])
	Child++;
      /* Percolate one level, gurantee it is not the last level*/
      if(LastElement>H->ElementType[Child])
	H->Elements[i]=H->Elements[Child];
      else 
	break;
    }
  H->Elements[i]=LastElement;
  return MinElement;
}
{% endhighlight %}
We can use the data structure in many problem. For example the input is a list of N elements, and an interger k. The selection problem is to find the kth largest element.

## Leftist Heaps
It seems difficult to design a data structure that efficiently supports merging and uses only an array, as in a binary heap. For this reason, all the advanced data structures that support efficient merging require the use of pointers. In practice, we can expect that this will make all the other operations slower. Like a binary heap, a *leftist heap* has both a structural peroperty and an ordering property. A *leftist heap* is also a binary tree, but the leftist heaps are not perfectly balanced but attempt to be very unbalanced. The *null path length* Npl(X) of any node Z to be the length of the shortest path from X to a node without two children. The Npl of a node with zero or one child is 0, while Npl(**NULL**)=-1. The *leftist heap* property is that for every node X in the node, the null path length of the left child is at least as large as that of the right child.

A *leftist tree* with r nodes on the right path must have at least 2^r-1 nodes.

The fundamental operation on leftist heaps is merging. To merge the two heaps, we compare their roots. Fist, we recursively merge the heap with the larger root with the right subheap of the heap with the smaller root. Then make this new heap the right child of the root of the heap with smaller root. Although the resulting heap statisfies the heap order property, it is may not leftist. It is easy to see that the remainder of the tree must be leftist. The right subtree of the root is leftist, because of the recursive step. The left subtree of the root has not been changed. Thus, we just need  to swap the root's left and right children and updating the null path length.
{% highlight objc %}
#ifndef _LeftHeap_H

struct TreeNode;
typedef struct TreeNode *PriorityQueue;

PriorityQueue Initialize(void);
ElementType FindMin(PriorityQueue H);
int IsEmpty(PriorityQueue H);
PriorityQueue Merge(PriorityQueue H1, PriorityQueue H2);

#dfeine Insert(X,H) (H=Insert1((X),H))
// Insert is a kind of special merge
PriorityQueue Insert1(ElementType X,PriorityQueue H);
PriorityQueue DeleteMin1(PriorityQueue H);

#endif
{% endhighlight}
The .c file is attached below:
{% highlight objc %}
struct TreeNode
{
  ElementType Element;
  PriorityQueue Left;
  PriorityQueue Right;
  int Npl;
};

PriorityQueue Merge(PriorityQueue H1, PriorityQueue H2)
{
  if(H1==NULL)
    return H2;
  if(H2==NULL)
    return H1;
  if(H1->Element<H2->Element)
    return Merge1(H1,H2);
  else
    return Merge1(H2,H1);
}

static PriorityQueue Merge1(PriorityQueue H1,PriorityQueue H2)
// H1->Element is smaller
{
  if(H1->left==NULL)
    H1->Left=H2;
  else
    {
      H1->Right=Merge(H1->Right,H2);
      if(H1->Left->Npl<H1->Right->Npl)
	SwapChildren(H1);
      H1->Npl=H1->Right->Npl+1;
    }
  return H1;
}

PriorityQueue Insert1(ElementType X,PriorityQueue H)
{
  PriorityQueue SingleNode;
  SingleNode=malloc(sizeof(struct TreeNode));
  if(SingleNode==NULL)
    FatalError("Out of space");
  else{
    SingleNode->Element=X; SingleNode->Npl=0;
    SingleNode->Left=SingleNode->Right=NULL;
    H=Merge(SingleNode,H);
  }
  return H;
}

PriorityQueue DeleteMin1(PriorityQueue H)
{
  PriorityQueue LeftHeap,RightHeap;
  if(IsEmpty(H))
    {
      Error("Priority queue is Empty");
      return H;
    }
  LeftHeap=H->Left;
  RightHeap=H->Right;
  free(H);
  return Merge(LeftHeap,RightHeap);
}
{% endhighlight %}

## Binomial Queues
*Binomial queues* differ from all the priority queue implementations that we have seen. It is a collection of heap-ordered trees, known as *forest*. Each of the heap-ordered trees is of a constrained form known as a *binomial tree*. There is at most one binomial tree of every height. A binomial tree of height 0 is a one-node tree; a binomial tree B_k of height k is formed by attaching a binomial tree, B_(k-1) to the root of another binomial tree B_(k-1).

*Merge* can be performed by merging the tree with same height from two forest. A *DeleteMin* can be performed by first finding the binomial tree with the smallest root. Let this tree be B_k and the original priority queue be H. We remove the B_k from the forest of trees in H, forming the new binomial queue H_1. We also remove the root of B_k, creating binomial trees B_0,B_1,B_2,...,B_(k-1), which collectively form priority queue H_2. We finish the operation by merging H_1 and H_2. These two kinds of operation are not easy to implement.
{% highlight objc %}
typedef struct BinNode *Position;
typedef struct Collection *BinQueue;

struct BinNode
{
  ElementType Element;
  Position LeftChild;
  Position NextSibling;
};

struct Collection
{
  int CurrentSize;
  BinTree TheTrees[MaxTrees];
};

BinTree CombineTrees(BinTree T1, BinTree T2)
{
  if(T1->Element>T2->Element)
    return CombineTrees(T2,T1);
  T2->NextSibling=T->LeftChild;
  T1->LeftChild=T2;
  return T1;
}


//H1 contain merged result
BinQueue Merge(BinQueue H1, BinQueue H2)
{
  BinTree T1,T2,Carry=NULL;
  int i,j;
  if(H1->CurrentSize+H2->CurrentSize>Capacity)
    Error("Merge would exceed capacity");
  H1->CurrentSize+=H2->CurrentSize;
  for(i=0,j=1;j<=H1->CurrentSize;i++,j*=2)
    {
      T1=H1->TheTrees[i]; T2=H2->TheTrees[i];
      //!! transform n>1 to 1 and 0 is still 0
      // so good skills and not easy to understand how to merge step by step
      // If only Carry, mean the length of merged result have no same length tree
      // in either H1 or H2. Carry should be resigned the value NULL
      switch(!!T1+2*!!T2+4*!!Carry)
	{
	case 0: /*no tree */
	case 1:/* only H1*/
	case 2:/* only H2*/
	case 4:/* only Carry */
	  H1->TheTrees[i]=Carry;
	  Carry=NULL;
	  break;
	case 3:/*H1 and H2*/
	  Carry=CombineTrees(T1,T2);
	  H1->TheTrees[i]=H2->TheTrees[i]=NULL;
	  break;
	case 5:/*H1 and Carry*/
	  Carry=CombineTrees(T1,Carry);
	  H1->TheTrees[i]=NULL;
	  break;
	case 6:/*H2 and Carry*/
	  Carry=CombineTrees(T2,Carry);
	  H2->TheTrees[i]=NULL;
	  break;
	case 7: /*All three */
	  H1->TheTrees[i]=Carry;
	  Carry=CombineTrees(T1,T2);
	  H2->TheTrees[i]=NULL;
	  break;
	}
    }
  return H1;
}

ElementType DeleteMin(BinQueue H)
{
  int i,j;
  int MinTree;
  BinQueue DeletedQueue;
  Position DeletedTree,OldRoot;
  ElementType MinItem;
  if(IsEmpty(H))
    {
      Error("Empty binomial queue");
      return H;
    }
  MinItem=Infinity;
  for(i=0;i<MaxTrees;i++)
    {
      if(H->TheTrees[i]&&H->TheTrees[i]->Element<MinItem)
	{
	  MinItem=H->TheTrees[i]->Element;
	  MinTree=i;
	}
    }
  DeletedTree=H->TheTrees[MinTree];
  OldRoot=DeletedTree;
  DeletedTree=DeletedTree->LeftChild;
  free(OldRoot);

  DeletedQueue=Initialize();
  DeletedQueue->CurrentSize=(1<<MinTree)-1;
  for(j=MinTree-1;j>=0;j--)
    {
      DeletedQueue->TheTrees[j]=DeletedTree;
      DeletedTree=DeletedTree->NextSibling;
      DeletedQueue->TheTrees[j]->NextSibling=NULL;
    }

  H->TheTrees[MinTree]=NULL;
  H->CurrentSize-=DeletedQueue->CurrentSize+1;

  Merge(H,DeletedQueue);
  return MinItem;
}
{% endhighlight %}
