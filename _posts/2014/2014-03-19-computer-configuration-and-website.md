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

In mac, the path is not easy to set
{%highlight objc %}
;;set up the path for the window-system
(defun set-exec-path-from-shell-PATH ()
    (let ((path-from-shell (shell-command-to-string "$SHELL -c 'echo $PATH'")))
      (setenv "PATH" path-from-shell)
      (setq exec-path (split-string path-from-shell path-separator))))
(when window-system (set-exec-path-from-shell-PATH))
(when window-system (cd "~/Desktop/"))
{%endhighlight objc %}

To install package, we can choose the **macport** package system to simplify our installation. [the flyspell script](http://www-sop.inria.fr/members/Manuel.Serrano/flyspell/flyspell.html) can be used to check word spelling when typing. And set different kinds of checking method below.

{%highlight objc %}
;; Use aspell for spell checking: brew install aspell --lang=en
(setq ispell-program-name "/opt/local/bin/aspell")
(setq ispell-program-name "/opt/local/bin/aspell")
(setq-default major-mode 'text-mode)
(add-hook 'text-mode-hook 'flyspell-mode)
(add-hook 'prog-mode-hook 'flyspell-prog-mode)
(add-hook 'LaTeX-mode-hook 'flyspell-mode)
(autoload 'flyspell-mode "flyspell" "On-the-fly spelling checker." t)
(autoload 'flyspell-delay-command "flyspell" "Delay on command." t)
(autoload 'tex-mode-flyspell-verify "flyspell" "" t) 
;;the last three function come from the script.
{%endhighlight objc %}

In emacs use **aspell** for word check, 
Update 2014-08-16 For emacs24, it provide the package and can be installed pretty easy. But the gs is hard to set in the os x. **PDF2DSC sentinel: Searching for program: No such file or directory, gs** , to solve the problem, set **.emacs Evaluate (setq preview-gs-options '("-q" "-dNOSAFER" "-dNOPAUSE" "-DNOPLATFONTS" "-dPrinted" "-dTextAlphaBits=4" "-dGraphicsAlphaBits=4")), tailoring to your setup. The important bit is changing "-dSAFER" to "-dNOSAFER"** through gui. In osx we need to install maxtex first.

I use [auctex](https://www.gnu.org/software/auctex/download-for-unix.html) in emacs for pdf and evince for preview. We can install the texlive-full first which is about 2GB and include nearly all packages.



I use [Ess](http://ess.r-project.org/index.php?Section=download) to run R code in emacs. Download from the link or install it directly through package.
	sudo apt-get install  ess
In mac, the ess have to be copied to .emacs.d and config.


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

[jedi.el](http://tkf.github.io/emacs-jedi/latest/) provides a fantastic way to auto complete python code and I can get doc using C-c ?.

## Website
[Find data in quandl](http://quandl.com)

[Competitions in data world-kaggle](http://kaggle.com)

