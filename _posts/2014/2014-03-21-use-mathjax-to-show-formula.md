---
layout: post
title: MathJax in Jekyll
categories:
- Programming
tags:
- Jekyll
---
To write math formula in the blog, we can use [Mathjax](http://mathjax.org).

* Change the head of html file.
In the blog system, change default.html under _layout dir.
{% highlight objc %}
<script type="text/javascript"
 src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
{% endhighlight %}

* Install kramdown for preview.
````
$gem install kramdown
````

* Setup the file  _config.yml in the blog system.
`markdown: kramdown`.
Now, write the formula in latex type.
- $$x_u=100$$
- $$e=\sum_{i=0}^{+\infty}\frac{1}{i!}$$
