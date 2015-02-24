---
layout: post
title: Keeping knitr happy after upgrading to R 3.0.0
tags:
- R
---
As noted [here](http://www.r-bloggers.com/r-3-0-0-is-released-whats-new-and-how-to-upgrade), after upgrading to R 3.0.0 you must run 

{% highlight r %}
update.packages(checkBuilt=TRUE)
{% endhighlight %}

This is because a bunch of packages have to be to rebuilt under R 3.0.0 in order to keep working. 
So I did, but that was not enough for [LyX](http://www.lyx.org/) to be able to compile my pdf's from [knitr](http://yihui.name/knitr/) like it used to only a week ago. What I had to do besides was this:

{% highlight r %}
remove.packages("tikzDevice")
install.packages("/Users/ghuiber/Downloads/tikzDevice_0.6.3.tar.gz", repos = NULL, type="source")
{% endhighlight %}

That is right. The package `tikzDevice` can no longer be installed directly from R-forge as a binary, as in

{% highlight r %}
install.packages("tikzDevice", repos="http://R-Forge.R-project.org")
{% endhighlight %}

Also, the source files are only available as a .tar.gz archive. To install from it on a Windows machine, you must have [Rtools](http://cran.r-project.org/bin/windows/Rtools/) installed first.
