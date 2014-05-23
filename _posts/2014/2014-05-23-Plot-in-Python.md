---
layout: post
title: Plot in Python
categories:
- Programming
tags:
- Python
- matplotlib

---
* matplotlib
* section2

---

## matplotlib
[tutorial](http://www.loria.fr/~rougier/teaching/matplotlib/#contour-plots)
Notes on some common used function in matplotlib. 
{% highlight objc %}
from matplotlib import pyplot as plt
##same as import pylab as plt
plt.scatter(x_array, y_array, c='set color here, b, r...',
	marker='marker for example o > +')
##this is the scatter plot
plt.plot(x, y)
## this is just for one point, if x and y are all number
plt.plot([x1, x2], [y1, y2], 'k--', lw=2)
## we can also set the color=(1,0,0)
##line from point x1 y1 to point x2 y2
##line width is 2 by using -- and color k
plt.fill_between([x0, x1], [y0, y1], [y2, y3], color=(1, 0, 0))
##color here is the rgb number
##the area on x is from x0 to x1
##y axis the first line from y0 to y2
## second line from y1 to y3
{% endhighlight objc %}
