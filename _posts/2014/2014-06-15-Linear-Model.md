---
layout: post
title: Linear Model
categories:
- machine learning
tags:
- Algorithm
- scikit

---
* Ridge Regression
* Logistic Regression Iris data
* KNN and Logistics to Digit data

---

## Ridge Regression
If there are few data per dimension, noise in the observations induces high variance, we use ridge regression to avoid this problem
{% highlight objc %}
import numpy as np
import pylab as pl
from sklearn import datasets
from sklearn import linear_model

X = np.c_[0.5, 1].T
y = [.5, 1]
test = np.c_[0 ,2].T
regr = linear_model.LinearRegression()
pl.figure(figsize=(6,4))
ax = pl.subplot(1, 2, 1)
np.random.seed(0)
for _ in range(6):
    this_X = 0.1*np.random.normal(size=(2,1)) + X
    regr.fit(this_X, y)
    ax.plot(test, regr.predict(test))
    ax.scatter(this_X, y, s=5, c='r')
ax.set_ylim([0,1])
regr = linear_model.Ridge(alpha = 0.1)
ax = pl.subplot(1, 2, 2)
for _ in range(6):
    this_X = 0.1*np.random.normal(size=(2,1)) + X
    regr.fit(this_X, y)
    ax.plot(test, regr.predict(test))
    ax.scatter(this_X, y, s=5, c='r')
ax.set_ylim([0,1])
pl.savefig('ridge.png')
{% endhighlight objc %}
The plot to compare regular linear regression and ridge regression

![ridge](/png/ridge.png)

## Logistic Regression Iris data
{% highlight objc %}
logistic = linear_model.LogisticRegression(C=1e5)
logistic.fit(iris_X_train, iris_y_train)

cmap_data = ListedColormap([(1,0.6,0.6),(0.6,1,0.6),(0.6,0.6,1)])
h = 0.02
x_min, x_max = iris_X[:,0].min()*0.9, iris_X[:,0].max()*1.1
y_min, y_max = iris_X[:,1].min()*0.9, iris_X[:,1].max()*1.1
xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                     np.arange(y_min, y_max,h))
Z = logistic.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)
pl.figure(figsize=(6,4))
pl.pcolormesh(xx, yy, Z, cmap=cmap_data)
{% endhighlight objc %}
![logistic regression](/png/logistic.png)

## KNN and Logistics to Digit data
{% highlight objc %}
from sklearn import metrics
#to use metrics.confusion_matrix to get the table
digits = datasets.load_digits()
X_digits = digits.data
y_digits = digits.target

np.random.seed(0)
indices = np.random.permutation(len(iris_X))
all = len(indices)
tenPercents = int(all*0.1)
digits_X_train = iris_X[indices[:-tenPercents]]
digits_y_train = iris_y[indices[:-tenPercents]]
digits_X_test = iris_X[indices[-tenPercents:]]
digits_y_test = iris_y[indices[-tenPercents:]]

clf = KNeighborsClassifier()
clf.fit(digits_X_train, digits_y_train)
print(metrics.confusion_matrix(clf.predict(digits_X_test),
                         digits_y_test))

clf = linear_model.LogisticRegression()
clf.fit(digits_X_train, digits_y_train)
print(metrics.confusion_matrix(clf.predict(digits_X_test),
                          digits_y_test))
{% endhighlight objc %}



