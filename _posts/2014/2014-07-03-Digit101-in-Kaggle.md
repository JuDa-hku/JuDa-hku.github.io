---
layout: post
title: Digit101-in-Kaggle
categories:
- Machine learning
tags:
- Scikit
- Matplotlib

---
* Kaggle digit 101

---

## Kaggle digit 101
!['Averaging image'](/png/meanDigit.png)
{% highlight objc %}
number = np.arange(0,10)
results = np.matrix(np.zeros(10*28*28)).reshape((10, 28*28))
for i in number:
    Xi_train = X_train[y_train==i,:]
    result = Xi_train.mean(0)
    results[i,:] = result
    
fig = pl.figure(figsize=(6,4))
for i in number:    
    a = results[i,:].reshape((28,28))
    pl.subplot(2,5,i+1)
    pl.xticks([])
    pl.yticks([])
    pl.imshow(a, cmap=cm.Greys)
pl.savefig('meanDigit.png')
{% endhighlight objc %}

