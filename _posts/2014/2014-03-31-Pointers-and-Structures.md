---
layout: post
title: Pointers and Structures
categories:
- Programming
tags:
- C

---
* How Memory is Allocated for a Structure
* Avoiding malloc/free Overhead
* Using Pointers to Support Data Structures
* Using Pointers to Support a Queue
* Using Pointer to Support a Stack
* Using Pointers to Support a Tree
* Summary

---

A structure's declaration frequently uses the **typedef** keyword to simplify its use.
{% highlight objc %}
typedef struct _person{
	char* firstName;
	char* lastName;
	char* title;
	unsigned int age;
} Person;
{% endhighlight %}

## How Memory is Allocated for a Structure
When a structure is allocated memory, the amount allocated to the stucture is at mimum the sum of the size of its individual fields. However, the size is often larger than its sum because padding can occur between fields of a structure. Several implications are related to this allocation of extra memory:

- Pointer arithmetic mush be used with care
- Arrays of structures may have extra memory between their elements

{% highlight objc %}
typedef struct _alternatePerson{
	char* firstName;
	char* lastName;
	char* title;
	unsigned int age;
} AlternatePerson;
{% endhighlight %}
Compare the size of these two structure.
{% highlight objc %}
Person person;
AlternatePerson otherPerson;
printf("%d\n",sizeof(person)); //displays 16
printf("%d\n",sizeof(otherPerson)); // displays 16
{% endhighlight %}

## Avoiding malloc/free Overhead
When structures are allocated and then deallocated repeatedly, some overhead will be incurred, resulting in a potentially significant performance penalty. One approach to deal with this problem is to maintain your own list of allocated structures. To demonstrate this approach we use **Person** structure. A pool of persons is maintainted in an array.
{% highlight objc %}
void deallocatePerson(Person *person){
	free(person->firstName);
	free(person->lastName);
	free(person->title);
}
#define LIST_SIZE 10
Person *list[LIST_SIZE];
void initializeList(){
	for(int i=0;i<LIST_SIZE;i++){
	list[i]=NULL;
	}
	}
Person *getPerson(){
for(int i=0;i<LIST_SIZE;i++){
if(list[i]!=NULL){
	Person *ptr=list[i];
	list[i]=NULL;
	return ptr;
}
}
	Person *person=(Person*)malloc(sizeof(Person));
	return person;
}
// either adds the person to the list or frees it up.
Person *returnPerson(Person *person){
	for(int i=0;i<LIST_SIZE;i++){
if(list[i]==NULL){
	list[i]=person;
	return person;
	}
}
	deallocatePerson(person);
	free(person);
	return NULL;
}
// the following illustrates the initialization of the list and adding a person to the list
initializeList();
Person *ptrPerson;
ptrPerson=getPerson();
initializePerson(ptrPerson,"Ralph","Fitsgerald","Mr.",35);
displayPerson(*ptrPerson);
returnPerson(ptrPerson);
{% endhighlight %}

## Using Pointers to Support Data Structures
We will examine four different data structures:

- Linked list: a singel-linked list
- Queue: A simple first-in first-out queue
- Stack: A simple stack
- Tree: A binary tree

The links connecting the nodes are easily implemented using a pointer. Each node can be dynamically allocated as needed. A Node structure is defined to represent a node. A pointer to **void** holds an arbitrary data type.
{% highlight objc %}
typedef struct_node{
	void *data;
	struct _node *next;
	} Node;

typedef struct _linkedList{
	Node *head;
	Node *tail;
	NOde *current;
} LinkedList;
void initializeList(LinkedList*) //Initializes the linked list
void addHead(LinkedList*,void*) //Adds data to the linked list's head
void addTail(LinkedList*,void*) //Adds data to the linked list's tail
void delete(LinkedList*,Node*) //Removes a node from the linked list
Node *getNode(LinkedList*,COMPARE,void*) //Returns a pointer to the node containing a specific data item
void displayLinkedList(LinkedList*,DISPLAY) //Displays the linked list
void initializeList(LInkedList *){
	list->head=NULL;
	list->tail=NULL;
	list->current=NULL;
}
void addHead(LinkedList *list,void* data){
	Node *node=(Node*)malloc(sizeof(Node));
	node->data=data;
	node->next=NULL;
	if(list->head==NULL){
		list->tail=node;
		node->next=NULL;
} else{
	node->next=list->head;
	}
	list->head=node;
}
void addTail(LinkedList *list,void* data){
	Node *node=(Node*)malloc(sizeof(Node));
	node->data=data;
	if(list->tail==NULL){
	list->head=node;
} else{
	list->tail->next=node;
	}
	list->tail=node;
}
// a case of using linked list
typedef struct _employee{
	char name[32];
	unsigned char age;
} Employee;
Linkedlist linkedList;
Employee *samuel=(Employee*) malloc(sizeof(Employee));
strcpy(samuel->name,"Samuel");
samuel->age=32;
Employee *sally=(Employee*) malloc(sizeof(Employee));
strcpy(sally->name,"Sally");
sally->age=28;
initializelist(&linkedList);
addHead(&linkedlist,samuel);
addHead(&linkedlist,sally);
{% endhighlight %}
The function's user probably has a pointer to the data but not to the node holding the data. To aid in identifying the node, a helper function has been provided to return a pointer to the node: **getNode**.
{% highlight objc %}
Node *getNode(Linkedlist* list, COMPARE compare, void* data){
Node *node=list->head;
while(node!NULL){
	if(compare(node->data,data)==0){
	return node;
	}
	node=node->next;
}
	return NULL;
}
{% endhighlight %}
The compare function illustrates using a function pointer at runtime to determine which function to use to perform a comparison. The **delete** follows.
{% highlight objc %}
void delete(LinkedList *list,Node *node){
	if(node==list->head){
		if(list->head->next==NULL){
			list->head=list->tail=NULL;
} else{
	list->head=list->head->next;
	}
}else{
	Node *tmp=list->head;
	while(tmp!=NULL&&tmp->next!=node){
		tmp=tmp->next;
}
	if(tmp!=NULL){
		tmp->next=node->next
}
	}
	free(node);
}
{% endhighlight %}
The **displayLinkedList** function illustrates how to traverse a linked list.
{% highlight objc %}
	void displayLinkedList(LinkedList *list, DISPLAY display){
		printf("\nLinked List\n");
		Node *current=list->head;
		while(current!=NULL){
			display(current->data);
			current=current->next
}
}
type def void(*DISPLAY)(void*);
void displayEmployee(Employee* employee){
	printf("%s\t%d\n",employee->name,employee->age);
}
addHead(&LinkedList,samuel);
addHead(&LinkedList,sally);
displayLinkedList(&LinkedList,(DISPLAY)displayEmployee);
{% endhighlight %}

## Using Pointers to Support a Queue
A queue is a linear data structure whose behavior is similar to a wating line. It typically supports two primary operations: enqueue and dequeue. The enqueue operation adds an element to the queue. The dequeue operation removes an element from the queue. Normally, the first element added to a queue is the first element dequeued from a queue. This behavior is referred to as First-In-First-Out(FIFO). To illustrate the queue, we will use the linked list developed before.
{% highlight objc %}
typedef LinkedList Queue;
void initializeQueue(Queue *queue){
	initializeList(queue);
	}
void equeue(Queue *queue, void *node){
	addHead(queue,node);
}
{% endhighlight %}
Three conditions are handled in **dequeue**, 1.An empty queue, 2.A single node queue, 3.A multiple node queue.
{% highlight objc %}
void *deuqeue(Queue *queue){
	Node* tmp=queue->head;
	void *data;
	if(queue->head==NULL){
		data=NULL;
	}else if(queue->head==queue->tail){
		queue->head=queue->tail=NULL;
		data=tmp->data;
		free(tmp);
}else{
	while(tmp->next!=queue->tail){
		tmp=tmp->next;
}
	queue->tail=tmp;
	tmp=tmp->next;
	queue->tail->next=NULL;
	data=tmp->data;
	free(tmp);
	}
	return data;
}
{% endhighlight %}

## Using Pointer to Support a Stack
The stack data structure is alos a type of list. In this case, elements are pushed onto the stack's top and then popped off. When multiple elements are pushed and the popped, the stack exhibits First-In-Last-Out(FILO) behavior.
{% highlight objc %}
typedef LinkedList Stack;
void initializeStack(Stack *stack){
	initializeList(stack);
	}
void push(Stack *stack, void *data){
	addHead(stack,data);
	}
void *pop(Stack *stack){
	Node *node=stack->head;
	if(node==NULL){
		return NULL;
	}else if(node=Stack->tail){
		stack->head=stack->tail=NULL;
		void *data=node->data;
		free(node);
		return data;
	}else{
		stack->head=stack->head->next;
		void *data=node->data;
		free(node);
		return data;
		}
}
{% endhighlight %}

## Using Pointers to Support a Tree
The tree is a very useful data structure whose name is derived from the relationship between its elements. Typically, child nodes are attached to a parent node. The overall form is an inverted tree where a root node represents the data structure's starting element. Pointers provide an obvious and dynamic way of maintaining the relationship between tree node.
{% highlight objc %}
typedef struct _tree{
  void *data;
  struct  _tree *left ;
  struct _tree *right;
} TreeNode;
void insertNode(TreeNode **root, COMPARE compare, void* data){
  TreeNode *node=(TreeNode*) malloc(sizeof(TreeNode));
  node->data=data;
  node->left=NULL;
  node->right=NULL;
  if(*root==NULL){
    *root=node;
    return;
  }
  while(1){
    if(compare((*root)->data,data)>0){
      if((*root)->left!=NULL){
	*root=(*root)->left;
      }else{
	(*root)->left=node;
	break;
      }
    }else{
      if((*root)->right!=NULL){
	*root=(*root)->right;
      }else{
	(*root)->right=node;
	break;
	}
    }
  }
}
typedef int(*COMPARE) (void*,void*);
int compareEmployee(Employee *e1, Employee *e2){
  return strcmp(e1->name,e2->name);
}
insertNode(&tree,(COMPARE) compareEmployee,samuel);
insertNode(&tree,(COMPARE) compareEmployee,sally);
{% endhighlight %}
There are serveral ways to display the nodes. The three orders are:

- In-order, Go left,visit the node, go right
- Pre-order, visit the node, go left, go right
- Post-order, Go left, go right, visit the node

{% highlight objc %}
void inOrder(TreeNode *root, DISPLAY display){
  if(root!=NULL){
    inOrder(root->left,display);
    display(root->data);
    inOrder(root->right,display);
  }
}
void postOrder(TreeNode *root, DISPLAY display){
  if(root!=NULL){
    postOrder(root->left,display);
    postOrder(root->right,display);
    display(root->data);
  }
}
void preOrder(TreeNode *root, DISPLAY display){
  if(root!=NULL){
    display(root->data);
    preOrder(root->left);
    preOrder(root->right);
  }
}
{% endhighlight %}

## Summary
The power and flexibility of pointers is exemplified when used to create and support data structures. Combined with dynamic memory allocation of structures, pointer enable the creation of data structures that use memory efficiently and can grow and shrink to meet the application's needs.


