---
layout: post
title: Words Count 
categories:
- Programming
tags:
- C
- C++
- Algorithm

---
* C++ with STL
* C Code (Read File Word by Word)

---

## C++ for Word Count
{% highlight objc %}
#include<iostream>
#include<fstream>
using namespace std;
#include<string>
#include<set>
#include<map>
int main(void)
{
  map<string,int> M;
  map<string,int>::iterator j;
  string t;
  fstream out;
  out.open("bible.txt",ios::in);
  while(out>>t)
    M[t]++;
  for(j=M.begin();j!=M.end();++j)
    cout<<j->first<<" "<<j->second<<"\n";
  return 0;
}
{% endhighlight objc %}
Using STL here makes the code short, but we may have the word like **the,** in the result.

## C Code for Word Count
We have to choose the data structure to store the result in C. Here, use hash function to translate the **word** to a integer and then count how many times the **word** shows in the example. First, we have to input the whole book by word, here I divide it as one word each line, then read each line one by one in the function **transfer()**. Second, choose the linked list to solve the problem that two words may have the same **key** in the hash table.
{% highlight objc %}
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<ctype.h>
#define NHASH 29989
#define MULT 31
typedef struct node *nodeptr;
typedef struct node{
  char *word;
  int count;
  nodeptr next;
} node;
nodeptr bin[NHASH];

void transfer()
{
  static const char filename[] = "bible.txt";
  static const char filename1[]="out";
  FILE *file = fopen(filename, "r");
  FILE *outfile=fopen(filename1,"w+");
  if ( file != NULL )
    {
      int ch, word = 0;
      while ( (ch = fgetc(file)) != EOF )
      {
         if ( isspace(ch) || ispunct(ch) )
         {
            if ( word )
            {
               word = 0;
	       fputc('\n',outfile);
	    }
         }
         else
         {
            word = 1;
	    fputc(tolower(ch),outfile);
	 }
      }
      fclose(file);
   }
   fclose(outfile);
}

unsigned int hash(char *p)
{
  unsigned int h=0;
  for(;*p;p++)
    {
    h=MULT*h+*p;
    }
  return h%NHASH;
}

void incword(char *s)
{
  unsigned int  h=hash(s);
  nodeptr p;
  for(p=bin[h];p!=NULL;p=p->next)
    if (strcmp(s,p->word)==0)
      {
      (p->count)++;
      return;
      }
  p=(nodeptr)malloc(sizeof(node));
  p->count=1;
  p->word=(char*)malloc(strlen(s)+1);
  strcpy(p->word,s);
  p->next=bin[h];
  bin[h]=p;
}


int main (void)
{
  transfer(); 
  for (int i=0;i<NHASH;i++)
    bin[i]=NULL;
  char *p;
  node* po;
  static const char filename1[]="out";
  static const char filename2[]="record";
  FILE *outfile1=fopen(filename1,"r+");
  FILE *outfile2=fopen(filename2,"w");
  while(fgets(p,20,outfile1)!=NULL)
    {
      incword(p);
    }
  fclose(outfile1);
  for (int i=0;i<NHASH;i++)
   {
    for(po=bin[i];po!=NULL;po=po->next)
      {
	fputs(po->word,outfile2);
	fprintf(outfile2,"%d\n",po->count);
	if(strcmp("the\n",po->word)==0){
  	printf ("%s %d",po->word,po->count);
	}
      }
      }
  fclose(outfile2);
  return 0; 
}
{% endhighlight objc %}
Pay attention **transfer** and the linked list in the code.
