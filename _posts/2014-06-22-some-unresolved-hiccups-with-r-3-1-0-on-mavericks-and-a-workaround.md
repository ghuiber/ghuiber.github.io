---
layout: post
title: Some unresolved hiccups with R 3.1.0 on Mavericks, and a workaround
---

If you're going to download the Mac binaries for the [latest R](http://cran.r-project.org/), you will see that they come in "Snow Leopard and higher" and "Mavericks and higher" flavors. If you run Mavericks, the latter is a natural choice, though the former clearly says "and higher" too, so it's got to be a valid option as well.

As it turns out, it's the better option, at least as of this writing.

The Mavericks build crashes with a segmentation fault upon attempting to load either the `caret` or `data.table` library, as reported [here](http://stackoverflow.com/questions/23107237/r-3-1-0-crashes-with-segfault-when-loading-data-table-package-1-9-2) and [here](http://stackoverflow.com/questions/24056670/unable-to-load-caret-package-in-r-on-r-3-1-0-on-osx-10-9). A brief search through the [R-SIG-Mac Archives](https://stat.ethz.ch/mailman/listinfo/r-sig-mac) returned no useful leads for fixing the problem.

Dropping the Mavericks build and installing the Snow Leopard one gave me back both `caret` and `data.table`. This works for me.

{% highlight r%}
> sessionInfo()
R version 3.1.0 (2014-04-10)
Platform: x86_64-apple-darwin10.8.0 (64-bit)

locale:
[1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
[1] RCurl_1.95-4.1   bitops_1.0-6     scales_0.2.4     ggplot2_1.0.0    reshape_0.8.5    data.table_1.9.2
[7] MASS_7.3-31     

loaded via a namespace (and not attached):
 [1] colorspace_1.2-4 digest_0.6.4     grid_3.1.0       gtable_0.1.2     htmltools_0.2.4  munsell_0.4.2   
 [7] plyr_1.8.1       proto_0.3-10     Rcpp_0.11.2      reshape2_1.4     rmarkdown_0.2.46 stringr_0.6.2   
[13] tools_3.1.0  
{% endhighlight %}
