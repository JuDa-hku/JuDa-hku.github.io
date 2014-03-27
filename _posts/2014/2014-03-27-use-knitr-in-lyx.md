---
layout: post
title: use knitr in lyx
categories:
- tools
tags:
- lyx
- kintr
---

* Introducation
* Setup
* A simple example

---

### 1.Introducation
[The web for kintr](yihui.name/knitr/),The knitr package was designed to be a transparent engine for dynamic report generation with R, solve some long-standing problems in Sweave, and combine features in other add-on packages into one package.


	Let us change our traditional attitude to the construction of programs:
	Instead of imagining that our main task is to instruct a computer what to do,
	let us concentrate rather on explaining to humans what we want the computer to do.
	                             Donald E.knuth, Literate Programming, 1984




For example, we have some data from other department to analysis every week. Once we finish the coding, Just insert them into lyx to get the lyx file. When we have the data, compile the lyx file to get the report whose result can be generated through the R code in the file.

### 2.Setup
[The setting for lyx ](yihui.name/knitr/demo/lyx)
There is a module named Rnw (knitr) in lyx now. To install the lastest lyx, use the ppa in ubuntu.
	sudo apt-get-repostitory ppa  
	sudo apt-get update
	sudo apt-get install lyx
Choose the Rnw(knitr) under Modules in the document setting. The knitr should be installed.


### 3.Simple example
{% highlight objc %}
<<set-potions,echo=FALSE,tidy=TRUE,cache=FALSE>>=
options(replace.assign=TRUE)
read_chunk('example.R')
@
we will insert R code here. In example.R,
code should have label such as ## @knitr try.
<<try>>=
@
{% endhighlight %}
The exmaple.R code can be
{% highlight objc %}
## @knitr try
runif(1)
{% endhighlight %}
Never forget to try you code in R first, else the lyx will spend lots of time to compile a wrong file. Read the log from lyx to debut the code. If your code is wrong, it may give you the message 'missing \begin{document}'.
