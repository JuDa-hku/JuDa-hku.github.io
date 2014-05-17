
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

Use [imaxima](https://sites.google.com/site/imaximaimath/download-and-install/easy-install-on-linux) for symbolic computation. It can even typeset the result of any calculation. [A short ref-card](http://hippasus.com/resources/symmath/maximacalc.html).
	factor(x^2+2*x+1);
	tex(%);
We can also output the result directly and create plain text file with mathmatical formula, click [here](https://sites.google.com/site/imaximaimath/imath-overview/tutorial-of-imath) for reference.

	m-x imath-mode %into the minor imath mode
	C-c-! %to complie
	C-c-& %to restore maxima code
	{maxima binomial(10,2) maxima} %in the plain text file


When visiting the saved file, if you place:
;; -*- mode: imath -*-
in the first line of the file, GNU Emacs automatically determines that the buffer needs imath mode, and enables it.

Use [Osterreichische Schulschrift](http://www.tug.dk/FontCatalogue/oesch/) and [Calligra](http://www.tug.dk/FontCatalogue/calligra/) for beatiful fonts. Calligra can be using directly by \uspackage{calligra} and {\calligra people come and go} after installing  texlive-fonts-extra package. Oesch is a little bit difficult to use. First find the texmf.cnf through

	sudo find / -type f -name "texmf.cnf"
	
to make sure /usr/local/share/texmf is TEXLOCAL:

	TEXMFLOCAL = /usr/local/share/texmf
	
Then put file in these place

	/usr/local/share/texmf/tex/latex/*.sty
	/usr/local/share/texmf/fonts/source/oesch/*.mf

Use python for machine learning study.

	git clone git://github.com/numpy/numpy.git numpy
	git clone git://github.com/scipy/scipy.git scipy
	git clone git://github.com/scikit-learn/scikit-learn.git
	sudo python setup.py build
	sudo python setup.py install
	apt-get install python-numpy python-scipy
	
Check the wesite for dependence packages, especially libatlas-dev and libatlas3gf-base.

## Website
[Find data in quandl](http://quandl.com)

[Competitions in data world-kaggle](http://kaggle.com)

