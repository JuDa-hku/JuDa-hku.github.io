---
layout: post
title: Phrases and Generating Text
categories:
- Programming
tags:
- C
- Algorithm

---
* The longest duplicated substring of characters
* Generating Text

---

## The longest duplicated substring of characters
We use a powerful data structure and apply it to a small problem: given an input file of text, find the longest duplicated substring of characters in it. For instance, the longest repeated string in "Ask not what your country can do for you, but what you can do for your country" is "can do for you". We'll use a simple data structure known as a "suffix array".
{% highlight c%}
#define MAXN 5000
char c[MAXN], *a[MAXN]
while(ch=getchar())!=EOF
	a[n]=&c[n]
	c[n++]=ch
c[n]=0
{% endhighlight%}
If a long string occurs twice in the array, it appears in two different suffixes. We will therefore sort the array to bring together equal suffixes. The "banana" array will be **banana,anana,nana,ana,na,a** and then are sorted to be **a,ana,anana,banana,na,nana**. We then use **comlen** function.
{% highlight c%}
qsort(a,n,sizeof(char *),pstrcmp)
for i=[0,n)
	if comlen(a[i],a[i+1])>maxlen
		maxlen=comlen(a[i],a[i+1])
		maxi=i
printf("%.*s\n",maxlen,a[maxi])
{% endhighlight c%}
The *pstrcmp* comparison function adds one level of indirection to the library *strcmp* function. This sxan through the array uses the *comlen* function to count the number of letters that two adjacent words have in common. 

## Generating Text
In Shannon's 1948 classic **Mathematical Theory of Communication**. Shannon writes, "To construct [order-1 letter-level text] for example, one opens a book at random and selects a letter at random on the page. This letter is recorded. The book is then opened to another page and one reads until this letter is encountered. The succeeding letter is then recorded. Turning to another page this second letter is searched for and the succeeding letter recorded, etc."

A program can automate this laborious task. Our C program to generate order-k Markov chains.
{% highlight c%}
int k=2
char inputchars[50000]
char *word[10000]
int nword=0
word[0]=inputchars
while scanf("%s",word[nword])!=EOF
	word[nword+1]=word[nword]+strlen(word[nword])+1
	nword++
{% endhighlight c%}
After we read the input, we will sort the **word** array to bring together all pointers that point to the same sequence of k words.
{% highlight c %}
int wordncmp(char *p,char *q)
	n=k
	for(;*p==*q;p++,q++)
		if(*p==0&&--n=0)
			return 0
	return *p-*q
{% endhighlight c%}
It scans through the two strings while the characters are euqal and returns equal after seeing k identical words. After reading the input, we append k null characters (so the comparison function doesn't run off the end), print the first k words in the document (to start the random output), and call the sort:
{% highlight c %}
for i=[0,k)
	word[nword][i]=0
for i=[0,k)
	print word[i]
qsort(word, nword, sizeof(word[0]),sortcmp)
{% endhighlight c%}
If k=1 and input text is "of the people, by the people, for the people", the word array might look like this: "by the", "for the", " of the", "people ", "people, for", "people, by", "the people,", "the people", "the people,". For clarity, this only shows the first k+1 words pointed to by each element by **word**. We may now generate nonsense text with this pseudocode sketch:
{% highlight c %}
phrase=first phrase in input array
loop
	perform a binary search for phrase in word[0..nword-1]
	for all phrases equal in the first k words
		select one at random, pointed to by p
	phrase=word following p
	if k-th word of phrase is length 0
		break
	print k-th word of phrase
phrase=inputchars
for(wordsleft=10000;wordsleft>0;wordsleft--)
	l=-1
	u=nword
	while l+1!=u
		m=(l+u)/2
		if wordncmp(word[m],phrase)<0
			l=m
		else
			u=m
	for (i=0; wordncmp(phrase,word[u+i])==0;i++)
		if rand()%(i+1)==0 //rand choose one that equal
			p=word[u+i]
	phrase=skip(p,1)
	if strlen(skip(phrase,k-1))==0
		break
	print skip(phrase,k-1)
{% endhighlight c %}
