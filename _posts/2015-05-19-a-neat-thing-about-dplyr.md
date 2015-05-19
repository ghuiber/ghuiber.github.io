---
layout: post
title: Here's a neat thing about dplyr
---

If you haven't yet read the [databases vignette](http://cran.r-project.org/web/packages/dplyr/vignettes/databases.html) of the dplyr package this may be news to you: dplyr does a kind of very fast in-database operations that can save you a lot of time.

[Where I work](http://www.windsorcircle.com/) we're a [PostgreSQL](http://www.postgresql.org/) shop, and that's a good thing for a data science operation that runs on R, because connecting to a database is a simple matter of calling `dplyr::src_postgres()`, once you have [RpostgreSQL](http://cran.r-project.org/web/packages/RPostgreSQL/index.html) running.

So now instead of loading the data into the RAM with the usual `DBI::dbFetch(DBI::dbSendQuery(...))` routine, and then munging it as needed, it's possible to leave it where it is and point to it with `dplyr::src()`. Then you can have dplyr pretend like it's doing all that `select()`, `filter()`, or `arrange()` stuff that it would if the data were in the RAM. It's just that in this case these operations don't actually change any data frames, because there aren't any data frames to change yet. Instead, they are translated as you go along into a gradually built SQL query. 

You can see what the query would do with `explain()` or `compute()`. The former shows the raw SQL; the latter hits the database for about then rows' worth, just enough for an example. The query is actually executed when you call `collect()` on it. That's when you get data into the RAM. Until then, you only get a `tbl`-class object that is a kind of promise.

This is fast: in one simple case, getting the raw data into RAM and munging it there took me 40 seconds, while building this promise took about 2. In the end, you can't avoid incurring the cost of actually moving data with `collect()`. But if the final set that you care about is small, `collect()` won't need to move much data, so the transfer will be quick.
  

