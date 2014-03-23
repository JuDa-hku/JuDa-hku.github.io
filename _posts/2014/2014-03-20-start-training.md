---
layout: post
<<<<<<< HEAD
title: For the try
categories:
- Programming
tags:
- C
- Algorithm
---

*介绍
=======
title: About markdown and math formula in pandoc
categories:
- Programming
tags:
- markdown
- emacs
- pandoc

---

* 介绍
* 在emacs使用pandoc并编译数学公式
>>>>>>> 08ba88a3473131293f4d931a3136dfb6e3850a17

---


### 1.introducation
<<<<<<< HEAD
[google us](http://google.us/)
>这样就引用啦
=======
[google us](http://google.us/)\\
> 这样就引用啦
>> 多次引用
>>>>>>> 08ba88a3473131293f4d931a3136dfb6e3850a17
*一个星是斜体*
**两个星是粗体**
---------
分割线
<<<<<<< HEAD
$$
\begin{equation}
x^2=3
\end{equation}
$$
参考方式定义链接:
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].
=======

$x^2=3$

参考方式定义链接:
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3]. 可以在要输入的代码前加tab来表示code输入.
>>>>>>> 08ba88a3473131293f4d931a3136dfb6e3850a17

	#include<stdio.h>
	int main()
	{    printf("hello Markdown");
	     return 0;
	}
也可以使用
<<<<<<< HEAD
{% highlight c %}
=======
{% highlight objc %}
>>>>>>> 08ba88a3473131293f4d931a3136dfb6e3850a17
#include<stdio.h>
int main(){
	printf("hello Markdown");
	return 0;
}
{% endhighlight %}

<<<<<<< HEAD
还可以使用
{% highlight objc %}
#include<stdio.h>
int main(){
	printf("hello Markdown");
	return 0;
}
{% endhighlight %}
=======
其中用

{% highlight objc %}
% highlight objc %
code here 
% endhighlight %
{% endhighlight %}
用{}将% highlight objec%括起来, highlight也一样.


### 在emacs使用pandoc并编译数学公式
- 安装emacs.
ubuntu 下非常简单.顺手装好ess.
{% highlight objc %}
sudo apt-get install emacs ess
{% endhighlight %}
- 安装markdown-mode和pandoc.
参考[刘思喆](http://www.bjt.name/2013/09/emacs-configure/)其中ubuntu源上的pandoc可能版本比较老，参考[pandoc](http://johnmacfarlane.net/pandoc/index.html)使用cabal安装，在我电脑老版本pandoc使用--mathjax失败.
在pandoc中写代码块可以写在两行长度大于三的~~~~~~之间.
- 修改markdown-mode的customization.
可以用'm-x customization-mode' 修改，将Markdown-command修改为支持mathjax的样式.
{% highlight objc %}
~/.cabal/bin/pandoc -f markdown -t html -s -c github.css --mathjax --highlight-style espresso
{% endhighlight %}
- C-c C-c p,是对md文件预览.
- C-c C-c m,在buffer看生成的代码.
- 在命令行直接生成html.
{% highlight objc %}
pandoc -s doc.md -o report.html --mathjax
{% endhighlight %}



>>>>>>> 08ba88a3473131293f4d931a3136dfb6e3850a17
  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"

