---
layout: post
title: Fresh RODBC troubles and a simple workaround on Yosemite
---

Today I upgraded to R 3.3.2, then clicked the "Update" button in RStudio. Everything was fine until RODBC again protested that it couldn't find `sql.h` and `sqlext.h` as documented [here](http://www.enoriver.net/2015/03/24/i-got-rodbc-to-work-on-a-mac/). 

The headers are in `/opt/local/include` if your ODBC driver manager is `unixODBC` and if you got it from MacPorts. All other headers RODBC needs are in `/usr/include`; `install.packages()` can see what's in `/usr/include` but for some reason it won't look in `/opt/local/include`. You might think that two symlinks would do the trick, but you don't need them. The fix is much simpler:

Dowload the source tarball, and instead of `install.packages("~/Downloads/RODBC_1.3-14.tar.gz", repos = NULL, type = "source")` at the R prompt just use `R CMD INSTALL RODBC_1.3-14.tar.gz` in Terminal. That was all I had to do. The latter found all the header files it needed, wherever they were.
