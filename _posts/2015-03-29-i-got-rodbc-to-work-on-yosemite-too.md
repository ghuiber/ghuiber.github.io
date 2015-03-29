---
layout: post
title: I got RODBC to work on Yosemite too
---

When I [got it going on Mavericks](http://www.enoriver.net/2015/03/24/i-got-rodbc-to-work-on-a-mac/) a few days ago I was left wondering if it would be equally easy on Yosemite. It's not much harder.

After you install `unixodbc` from Macports, you just need to edit your `~/.Rprofile` file to make `install.packages()` aware of the include and library paths that RODBC needs. My edits are as follows:

```
# This is where unixODBC keeps things that RODBC needs
Sys.setenv(ODBC_INCLUDE="/opt/local/include")
Sys.setenv(ODBC_LIBS="/opt/local/lib")
```

After this, restart R and just do

{% highlight r %}
install.packages("~/Downloads/RODBC_1.3-11.tar.gz", repos = NULL, type = "source")
{% endhighlight %}

