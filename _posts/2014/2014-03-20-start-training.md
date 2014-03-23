---
layout: post
title: About markdown and math formula in pandoc
categories:
- Programming
tags:
- C
- Algorithm
---
* 介绍
* 在emacs使用pandoc并编译数学公式

---

### 1.introducation
[google us](http://google.us/).

 > This is a block quote.
 >
 >> Second level
 > > > gong
 > back to first level

*一个星是斜体*,
**两个星是粗体**,
___________,
*********
分割线
$$
\begin{equation}
x^2=3.
\end{equation}
$$
参考方式定义链接:
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

	#include<stdio.h>
	int main()
	{    printf("hello Markdown");
	     return 0;
	}
也可以使用
{% highlight objc %}
% highlight objc %
#include<stdio.h>
int main(){
	printf("hello Markdown");
	return 0;
}
% endhighlight %
{% endhighlight %}
其中
{% highlight objc %}
% highlight objc %
code here 
% endhighlight %
{% endhighlight %}
用{}将% highlight objc%括起来.


### 2.在emacs使用pandoc并编译数学公式
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




  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"

