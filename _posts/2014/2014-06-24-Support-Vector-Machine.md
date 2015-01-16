---
layout: post
title: Support Vector Machine
categories:
- Machine Learning
tags:
- Scikit

---
* SVM Example 
* section2
---

## SVM Example
SVM is a very useful tool for large dataset, its training process is quite fast. KKT condition is important when deducing the algorithm.
{% highlight python %}
import numpy as np
import pylab as pl
from sklearn import datasets
from sklearn import svm
from matplotlib.colors import ListedColormap
iris = datasets.load_iris()
X = iris.data
y = iris.target
X = X[:,:2]
C = 1.0
svc = svm.SVC(kernel='linear', C=C).fit(X,y)
rbf_svc = svm.SVC(kernel='rbf',gamma=0.7, C=C).fit(X,y)
poly_svc = svm.SVC(kernel='poly', degree=3, C=C).fit(X,y)
lin_svc = svm.LinearSVC(C=C).fit(X,y)
cmap_data = ListedColormap([(1,0.6,0.6),(0.6,1,0.6),(0.6,0.6,1)])
h = 0.01
x_min, x_max = X[:,0].min()*0.9, X[:,0].max()*1.2
y_min, y_max = X[:,1].min()*0.9, X[:,1].max()*1.2
xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                     np.arange(y_min, y_max,h))

titles = [ 'SVC with linear kernel',
            'SVC with RBF kernel',
           'SVC with polynomial (degree 3) kernel',
           'LinearSVC']
pl.figure(figsize=(6,4))
for i, clf in enumerate((svc, rbf_svc, poly_svc, lin_svc)):
    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    
    pl.subplot(2,2,i+1)
    pl.axis('off')
    #pl.pcolormesh(xx, yy, Z, cmap=cmap_data)
    pl.contourf(xx, yy, Z, cmap=pl.cm.Paired)
    pl.scatter(X[:,0], X[:,1], c=y, cmap=cmap_data)
    pl.title(titles[i])

pl.savefig('svm_iris.png')
{% endhighlight python %}
![svm_iris.png](/png/svm_iris.png)
