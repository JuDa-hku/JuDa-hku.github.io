---
layout: post
title: Mistakes I have made
categories:
- Programming

tags:
- C


---
* Assignment
* section2
---

## Assignment
I forget to write the keyword *return* in a recursion program.
Once, I put cout<<result<<endl behind **return true**, and wonder where is my result!!
Destructor should be carefully written and it may cause segmentation fault. In computing, a segmentation fault or access violation is a fault raised by hardware with memory protection (From wiki).
In c++, if you want to initialize a struct pointer, use the key word **new** to get enough space, else it may raise segmentation fault.  
*account* ac = new account*

When free the space recursively, do not forget **delete** the root.
	freeTree(root->right);
	freeTree(root->left);
	delete root;

Why I cannot define Set<Set<int> > in c++?

