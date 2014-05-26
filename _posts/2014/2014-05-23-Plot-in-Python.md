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
* figures, subplots, axes and ticks
* plot and fill_between
* scatter and color vector
* bar plot and add text
* contour and savefig
* imshow with colorbar()
* pie
* grid
* multi plots
* polar axis with subplots
* matplotlib logo

---

## matplotlib
The most important part is to tell the connections between classes, for example class axes, class axis, class fig.
[tutorial](http://www.loria.fr/~rougier/teaching/matplotlib/#contour-plots)
Notes on some common used function in matplotlib. 
{% highlight objc %}
from matplotlib import pyplot as plt
##same as import pylab as plt
plt.scatter(x_array, y_array, c='set color here, b, r...',
	marker='marker for example o > +')
##this is the scatter plot
plt.plot(x, y)
v## this is just for one point, if x and y are all number
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
A example to use different colors to show different area.
{% highlight objc %}
import numpy as np
from matplotlib import colors
# colors can set different color for each label
import pylab as pl
x = np.linspace(0, 1, 100)
y = np.linspace(0, 1, 100)
X, Y = np.meshgrid(x, y)
## mesh for plot
label = np.concatenate(([0]*100*33, [1]*100*33, [2]*100*34))
labelmatrix = label.reshape(np.array([100, 100]))
## set label for each point, this is one to one
cmp = colors.ListedColormap([(1, 0 ,0), (0, 1, 0), (0, 0, 1)])
pl.pcolormesh(X, Y, labelmatrix, cmap=cmp)
## according to label, points have different color
pl.show()
{% endhighlight %}

## figures, subplots, axes and ticks
A **figure** in matplotlib means the whole window in the user interface. Within this figure there can be "subplots". When we call plot, matplotlib calls *gca()* to get the current axes and gca in turn calls *gcf()* to get the current figure. If there is none it calls *figure()* to make one, strictly speacking, to make a *subplot(111)*

- num *1*  number of figure
- figsize *figure.figsize* figure size in inches
- dpi *figure.dpi* resolution in dots per inch
- facecolor *figure.facecolor* color of the drawing background
- edgecolor *figure.edgecolor* color of edge around the drawing background
- frameon *True* draw figure frame or not
- pl.close(1) *close figure 1*

[Grid](http://matplotlib.org/users/gridspec.html) command is a more powerful alternative for subplots.

Axes are very similar to subplots but allow placement of plots at any location in the figure. So if we want to put a smaller plot inside a bigger one, use axes.

Well formatted ticks are important. Tick locators control the positions of the ticks. Major and minor ticks can be located and formatted independently from each other. Per default minor ticks are not shown.  They are set as
	ax = pl.gca()
	ax.xaxis.set_major_locator(eval(locator))


## plot and fill_between
{% highlight objc %}
import numpy as np
import pylab as pl
n = 256
X = np.linspace(-np.pi, np.pi, n, endpoint=True)
Y = np.sin(2*X)
pl.axes([0.025, 0.025, 0.95, 0.95])
pl.plot(X, Y+1, c='b', alpha=0.5)
pl.fill_between(X, 1, Y+1, Y<0, color='blue', alpha=0.25)
pl.fill_between(X, 1, Y+1, Y>0, color='red', alpha=0.25)
pl.plot(X, Y-1,color='blue', alpha=1.00)
pl.fill_between(X, -1,Y-1, where=(Y>0), color='blue', alpha=0.25 )
pl.fill_between(X, -1,Y-1, where=(Y<0), color='red', alpha=0.25 )
pl.xlim(-np.pi, np.pi)
pl.xticks([])
pl.ylim(-2.5,2.5)
pl.yticks([])
pl.show()
{% endhighlight objc %}

## scatter and color vector
{% highlight objc %}
import numpy as np
import pylab as pl
n = 1024
X = np.random.normal(0,1,n)
Y = np.random.normal(0,1,n)
T = np.arctan2(Y,X)
##set the color vector 
pl.axes([0.025, 0.025, 0.95, 0.95])
# axes(rect, axisbg='w') rect=[left, bottom, width, height]
#in normalized(0,1) units. axisbg is the background color default white
pl.scatter(X, Y,s=75, c=T, alpha=0.5)
pl.xlim((-1.5, 1.5))
pl.ylim(-1.5, 1.5)
pl.xticks([])
pl.yticks([])
pl.show()
{% endhighlight %}

## bar plot and add text
{% highlight objc %}
import numpy as np
import pylab as pl
n = 12
X = np.arange(n)
Y1 = (1-X/float(n)) * np.random.uniform(0.5,1.0,n)
Y2 = (1-X/float(n)) * np.random.uniform(0.5,1.0,n)

pl.bar(X, +Y1, facecolor='b', edgecolor='white')
pl.bar(X, -Y2, facecolor='pink', edgecolor='white')

for x,y in zip(X,Y1):
    pl.text(x+0.4, y+0.05, '%.2f' % y, ha='center', va= 'bottom')
for x,y in zip(X,Y2):
    pl.text(x+0.4,-y-0.05, '%.2f'%y,ha='center', va='top')
#ha means horizontalalignment [center,top,bottom,baseline]
#va means verticalaligment
pl.xlim（-0.5,n)
pl.xticks([])
pl.ylim(-1.25,+1.25)
pl.yticks([])
pl.show()
{% endhighlight object %}

## contour and savefig
Refer [here](http://matplotlib.org/examples/color/colormaps_reference.html) for more color_maps.
{%highlight object %}
import pylab as pl
import numpy as np

def f(x,y): return (1-x/2+x**5+y**3)*np.exp(-x**2-y**2)

n = 256
x = np.linspace(-3,3,n)
y = np.linspace(-3,3,n)
X,Y = np.meshgrid(x,y)

pl.contourf(X, Y, f(X,Y), 8, alpha=.75, cmap=pl.cm.hot)

pl.C = pl.contour(X, Y, f(X,Y), 8, colors='black', linewidth=.5)
pl.clabel(pl.C, fontsize=10, inline=1)
pl.savefig('./contour.png')
pl.show()
{% endhighlight object %}

## imshow with colorbar()
{% highlight objct %}
import pylab as pl
def f(x,y):
    return (1-x/2+x**5+y**3)*np.exp(-x**2-y**2)

n = 10
x = np.linspace(-3,3,4*n)
y = np.linspace(-3,3,3*n)
X,Y = np.meshgrid(x,y)
pl.imshow(f(X,Y), cmap='bone', interpolation='nearest', origin='lower')
##origin set the [0,0] index of the array to upper left or lower left
##interpolation has many choice and will show in ps, or pdf.
pl.colorbar()
pl.show()
{% endhighlight object %}

## pie
{% highlight object %}
from matplotlib import pyplot as pl
import numpy as np
n = 3
Z = np.ones(n)
Z[-1] = 0.2
# we can also set figure and axes equal first
#figure(figsize=(8,8))
#ax = axes([0.1, 0.1, 0.8, 0.8])
# or axes(aspect='equal')
pl.axes([0.025, 0.025, 0.95, 0.95])
colors = ['0.5',' 0.5', '0.5']
pl.pie(Z,colors=colors, explode=[0,0,0.2], labels=('man','woman','not known'), autopct='%1.1f%%')
# explod set the distance of each piece to the center
# autopct add the percent of each piece
# label add name
pl.gca().set_aspect('equal')
pl.show()
{% endhighlight object %}
*gca()* return the current axes, creating one if necessary.
By *set_aspect('equal')* figure and axes are euqal, the pie chart look best .
**axes(aspect=1)** is the easiest way.

##grid
{% highlight object %}
import pylab as pl
ax = pl.axes([0.025, 0.025, 0.95, 0.95])
##axis classes for the ticks and x and y axis
ax.set_xlim(0,4)
ax.set_ylim(0,3)
ax.xaxis.set_major_locator(pl.MultipleLocator(1.0))
ax.xaxis.set_minor_locator(pl.MultipleLocator(0.1))
ax.yaxis.set_major_locator(pl.MultipleLocator(1.0))
ax.yaxis.set_minor_locator(pl.MultipleLocator(0.1))
## axes girds on or off
##axes.grid(b=None, which='major', axis='both', **kwargs)
ax.grid(which='major', axis='x', linewidth=0.75, linestyle='-', color='0.75')
ax.grid(which='minor', axis='x', linewidth=0.25, linestyle='-', color='0.75')
ax.grid(which='major', axis='y', linewidth=0.75, linestyle='-', color='0.75')
ax.grid(which='minor', axis='y', linewidth=0.25, linestyle='-', color='0.75')
ax.set_xticklabels([])
ax.set_yticklabels([])
pl.show()
{% endhighlight object%}

## Multi plots
{% highlight object %}
import pylab as pl
fig = pl.figure()
fig.subplots_adjust(bottom=0.025, left=0.025, top=0.975, right=0.975)
## in the class figure
pl.subplot(2,1,1)
pl.xticks([])
pl.yticks([])
pl.subplot(2,3,4)
pl.xticks([])
pl.yticks([])
pl.subplot(2,3,5)
pl.xticks([])
pl.yticks([])
pl.subplot(2,3,6)
pl.xticks([])
pl.yticks([])
pl.show()
{% endhighlight object %}

## polar axis with subplots
{% highlight object %}
import pylab as pl
import numpy as np
#ax = pl.axes([0,0,1,1], polar=True)
ax = pl.subplot(2,2,2, polar=True)
N=20
theta = np.arange(0, 2*np.pi, 2*np.pi/N)
radii = 10*np.random.rand(N)
width = np.pi/4*np.random.rand(N)
bars = ax.bar(theta, radii, width=width, bottom=0)
for r,bar in zip(radii, bars):
    bar.set_facecolor(pl.cm.jet(r/10))
    bar.set_alpha(0.5)
ax.set_xticklabels([])
ax.set_yticklabels([])
pl.axes([0,0,0.5,0.5])
bars = pl.bar(theta, radii, width=width, bottom=0.0)
for r,bar in zip(radii, bars):
    bar.set_facecolor(pl.cm.jet(r/10))
    bar.set_alpha(0.5)
pl.show()





{% endhighlight object}





## 3d plot
import pylab as pl
import numpy as np
from mpl_toolkits.mplot3d import Axes3D

fig = pl.figure()
ax = Axes3D(fig)
X = np.arange(-4, 4, 0.25)
Y = np.arange(-4, 4, 0.25)
X, Y = np.meshgrid(X, Y)
R = np.sqrt(X**2 + Y**2)
Z = np.sin(R)

ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap='hot')
ax.contourf(X, Y, Z, zdir='z', offset=-2,cmap=cm.hot)
ax.set_zlim(-2,2)
pl.show()


## matplotlib logo design
The logo are composed by the background text, the bar plot and the text "matplotlib". Plot each part one by one.
{% highlight object %}
# from Tony Yu  logo design for matplotlib
from matplotlib import pyplot as plt
import numpy as np
import matplotlib as mpl
import matplotlib.cm as cm

mpl.rcParams['xtick.labelsize'] = 10
mpl.rcParams['ytick.labelsize'] = 12
mpl.rcParams['axes.edgecolor'] = 'gray'

axalpha = 0.05
figcolor = 'white'
dpi = 80
fig = plt.figure(figsize=(6, 1.1),dpi=dpi)
fig.figurePatch.set_edgecolor(figcolor)
fig.figurePatch.set_facecolor(figcolor)

def add_math_background():
    ax = fig.add_axes([0, 0, 1, 1])

    text = []
    text.append((r"$W^{3\beta}_{\delta_1 \rho_1 \sigma_2} = U^{3\beta}_{\delta_1 \rho_1} + \frac{1}{8 \pi 2} \int^{\alpha_2}_{\alpha_2} d \alpha^\prime_2 \left[\frac{ U^{2\beta}_{\delta_1 \rho_1} - \alpha^\prime_2U^{1\beta}_{\rho_1 \sigma_2} }{U^{0\beta}_{\rho_1 \sigma_2}}\right]$", (0.7, 0.2), 20))
    text.append((r"$\frac{d\rho}{d t} + \rho \vec{v}\cdot\nabla\vec{v} = -\nabla p + \mu\nabla^2 \vec{v} + \rho \vec{g}$",
                (0.35, 0.9), 20))
    text.append((r"$\int_{-\infty}^\infty e^{-x^2}dx=\sqrt{\pi}$",
                (0.15, 0.3), 25))
    #text.append((r"$E = mc^2 = \sqrt{{m_0}^2c^4 + p^2c^2}$",
    #            (0.7, 0.42), 30))
    text.append((r"$F_G = G\frac{m_1m_2}{r^2}$",
                (0.85, 0.7), 30))
    for eq, (x, y), size in text:
        ax.text(x, y, eq, ha='center', va='center', color="#11557c", alpha=0.25, transform=ax.transAxes, fontsize=size)
    ax.set_axis_off()
    return ax

def add_matplotlib_text(ax):
    ax.text(0.95, 0.5, 'matplotlib', color='#11557c', fontsize=65,
    ha='right', va='center', alpha=1.0, transform=ax.transAxes)

def add_polar_bar():
    ax = fig.add_axes([0.025, 0.075, 0.2, 0.85], polar=True)

    ax.axesPatch.set_alpha(axalpha)
    ax.set_axisbelow(True)
    N = 7
    arc = 2*np.pi
    theta = np.arange(0, arc, arc/N)
    radii = 10 * np.array([0.2, 0.6, 0.8, 0.7, 0.4, 0.5, 0.8])
    width = np.pi/4*np.array([0.4, 0.4, 0.6, 0.8, 0.2, 0.5, 0.3])
    bars = ax.bar(theta, radii, width=width, bottom=0.0)
    for r, bar in zip(radii, bars):
        bar.set_facecolor(cm.jet(r/10.))
        bar.set_alpha(0.6)
    for label in ax.get_xticklabels() + ax.get_yticklabels():
        label.set_visible(False)
        

    for line in ax.get_ygridlines() + ax.get_xgridlines():
        line.set_lw(0.8)
        line.set_alpha(0.9)
        line.set_ls('-')
        line.set_color('0.5')


        ax.set_yticks(np.arange(1, 9 ,2))
        ax.set_rmax(9)

if __name__ == '__main__':
    main_axes = add_math_background()
    add_polar_bar()
    add_matplotlib_text(main_axes)
    print("main")
    plt.savefig("./try.png")
    plt.show()
{% endhighlight object %}






















