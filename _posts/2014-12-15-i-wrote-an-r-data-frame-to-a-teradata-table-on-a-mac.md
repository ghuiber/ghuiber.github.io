---
layout: post
title: I wrote an R data frame to a Teradata table on a Mac
---

Here's how I did it:

1. On a new Mac running Mavericks and R 3.1.2 with [devtools](https://github.com/hadley/devtools) I installed [Java for Mac](http://support.apple.com/kb/DL1572).
2. I installed the RJDBC package from CRAN (which depends on the DBI package also from CRAN) and the [teradataR](https://github.com/jeffwong/teradataR) package from GitHub.
3. I downloaded the [Teradata JDBC driver](https://downloads.teradata.com/download/connectivity/jdbc-driver), unpacked it, and moved `tdgssconfig.jar` and `terajdbc4.jar` to `/System/Library/Java/Extensions`.

After that, writing the data frame `foo` to the table `DATABASE.BAR` was as simple as:

{% highlight r %}
teradataR::tdConnect(dsn='datamart.mycompany.com',
                     uid='user',pwd='pass', 
                     dType='jdbc')
teradataR::tdWriteTable(databasename='DATABASE', 
                        tablename='BAR', df=foo)
teradataR::tdClose(conn)
{% endhighlight %}

I had to do this because `DBI::dbWriteTable()` now fails on Teradata as explained [here](http://stackoverflow.com/questions/26769364/writing-a-data-frame-to-a-teradata-table-using-rjdbc).

My thanks go to [Jeffrey Wong](http://jeffwong.github.io/) for mirroring and nurturing the [no-longer-supported teradataR package](http://downloads.teradata.com/download/applications/teradata-r), and to [Skylar Lyon](http://about.me/skylarlyon) for finding Jeff's repo.
