---
layout: post
title: Computer Configuration and Website
categories:
- Programming
tags:
- configuration

---
* Computer Configuration
* Website

---

## Computer Configuration
I use [auctex](https://www.gnu.org/software/auctex/download-for-unix.html) in emacs for pdf and evince for preview.
{% highlight objc %}
(setq TeX-PDF-mode t)
(defun pdfevince ()
   (add-to-list 'TeX-output-view-style
                 '("^pdf$" "." "evince %o %(outpage)")))
(add-hook  'LaTeX-mode-hook  'pdfevince  t) ; AUCTeX LaTeX mode
{% endhighlight %}

I use [Ess](http://ess.r-project.org/index.php?Section=download) to run R code in emacs. Download from the link or install it directly through package.
	sudo apt-get install  ess

Use [imaxima](https://sites.google.com/site/imaximaimath/download-and-install/easy-install-on-linux) for symbolic computation.


## Website
[Find data in quandl](http://quandl.com)
[Competitions in data world-kaggle](http://kaggle.com)

