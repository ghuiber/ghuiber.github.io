---
layout: post
title: I got RODBC to work on a Mac (OS X Mavericks)
---

All I had to do was use my MacPorts: after I ran `sudo port install unixodbc` in the Terminal, I ran again

{% highlight r %}
install.packages("~/Downloads/RODBC_1.3-11.tar.gz", repos = NULL, type = "source")
{% endhighlight %}

at the R console and it worked fine.

Weirdest thing. I distinctly remember that this used to be a problem. That is why I set up RJDBC in the first place. My previous attempts at installing RODBC on a Mac failed with the [well-documented missing header errors](http://comments.gmane.org/gmane.comp.lang.r.db/953):

{% highlight r %}
checking sql.h usability... no
checking sql.h presence... no
checking for sql.h... no
checking sqlext.h usability... no
checking sqlext.h presence... no
checking for sqlext.h... no
configure: error: "ODBC headers sql.h and sqlext.h not found"
ERROR: configuration failed for package ‘RODBC’
* removing ‘/Users/ghuiber/Rlibs/RODBC’
{% endhighlight %}

The headers were there alright, in `/opt/local/include`; it's just that the RODBC installer couldn't find them, no matter where else I tried to symlink them. 

Now I wonder if this fix also works on Yosemite.


 



