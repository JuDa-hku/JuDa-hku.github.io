---
layout: post
title: Data Science
categories:
- Programming
- Machine Learning
- Tools
tags:
- Markdown
- Algorithm
- C
- Scikit
- Python

---
* Data Visualization
* Bayesian and Frequentist
* section2

---

Click [here](https://github.com/jakevdp/sklearn_pycon2013) for a  nice introduction ipython notebook of the sklearn package.

## Data Visualization
I choose the picture from web to show which method we should use to visualize our data. We should never use a 3-D plot to visualize data and instead use color or shape to mean other dimension. We often think a rainbow is a good choice for color map but it is in fact not so effective as many other color map provided in brewer2mpl. This cool [website](http://colorbrewer2.org/) shows kinds of color map which has been proved useful for sequential qualitative or diverging data.  
![Visualization](/png/visualization.png?raw=true)  
In this case, we need to show the rainfall for each province and I choose color to visualize the number. It is sequential data and we can need one color map from the website.  
![RainFall](/png/2012RainFall.png?raw=true)
Use *fill* function provided in the matplotlib, and choose colormap from brewer2mpl. We can fill the area and choose the color according to some number by using Normalize function in mpl.colors.


## Bayesian and Frequentist
- Bayesian: Assume the parameters that we are interested in have some kinds of distribution. We use $$\pi(\theta \vert y)$$ to denote the posterior distribution. If we consider the posterior distribution, it is not hard to find the connection between the distribution and likelihood function. If we consider interval estimation, we can find the interval, e.g. $$P(\theta \in (-2,5) \vert y)=0.95$$, because we can calculate the posterior distribution for the parameter. Common prior distribution to choose is conjugate priors to simplify our calculation. But due to the fast computers we have, we use MCMC to simulate the procedure.

- Frequentist: Never use $$\pi(\theta \vert y)$$. They tend to assume there are given value for the parameter we want to estimate. The $$\theta$$ should be fixed and unknown. When it refer to confidence interval, for example 95% confidence, we cannot have the same notation as Bayesian and it does not mean that the parameter will lie in this interval with probability 0.95. In fact, it means that it is 95% true that the interval will cover the true parameter. We can treat the CI as some long time estimation, for example FDA will check thousands of drugs every year and they want to control the error rate.

##Interesting trick
- Consider the regression line y=cx and x=ty. if dataset are normalized and standardized, c and t should be the same even though according to math c should be 1 over t. This is called regression to the mean.
- Data in high dimensional space tends to lie in the boundary of the circle. An intuitive explanation is that as long as any component of the random vector near 1, the square sum will near 1 which means that the point lies in the boundary. The volume of a circle  over the volume of a cube tends to decrease when the dimension d increase.
- Example, we have 100 sample and each sample has 1000 feature. We want to choose a model using cross-validation. Should we select features first or select features within the CV process? Do variable selection in the CV process, because if we do variable selection first according to the whole dataset, the error rate will be trick by using CV because these features are decided by the whole dataset.
- Think like a Bayesian. When you see hear someone speak cantonese ? Do you think where he come from ?  From HongKong or not ? Please think like a Bayesian ~. Or when you watch a high level football game and the team you support has been defeated by 0:7 and you may think they even do not play as well as I do. What is the possibility of this event ?
