---
layout: post
title: Date vs. POSIXct
---

This post is about a quirk in the way R handles dates.

I have a data set, `rfm`, which has a column named `cohort`, of class `Date` as shown below. I also have a vector of calendar dates, rather cleverly named `dates`, whose elements are also of class `Date`:

```
> class(rfm)
[1] "tbl_df"     "data.frame"
> class(rfm$cohort)
[1] "Date"
> class(dates)
[1] "Date"
``` 

Both `rfm$cohort` and `dates` show the date of the first of the month over a certain time period, with the possibility that `dates` covers a more recent set of months. My problem is simple: I just want to see how many months there are between `max(rfm$cohort)` and `max(dates)`.

The [`lubridate`](https://cran.r-project.org/web/packages/lubridate/) package makes this easy with the `interval()` function, but the arguments of that function must be `POSIXct`, not `Date` objects, although this will not be readily apparent:

```
> interval(max(rfm$cohort),max(dates))
[1] 2015-05-31 20:00:00 EDT--2015-06-30 20:00:00 EDT
```  

See? No error message whatsoever. Let's take a closer look:

```
> str(interval(max(rfm$cohort),max(dates)))
Formal class 'Interval' [package "lubridate"] with 3 slots
  ..@ .Data: num 2592000
  ..@ start: POSIXct[1:1], format: "2015-05-31 20:00:00"
  ..@ tzone: chr ""
```

Well, there's no time zone, but other than that this seems to work. Except that it does not:

```
> as.period(interval(max(rfm$cohort),max(dates)), months)
Error in while (any(start + est * per < end)) est[start + est * per <  : 
  missing value where TRUE/FALSE needed
```

This is not all that informative, but what if we recast the two arguments as POSIXct objects?

```
> as.period(interval(ymd(as.character(max(rfm$cohort))),ymd(as.character(max(dates)))), months)
[1] "1m 0d 0H 0M 0S"
```

This worked! But why?

```
> str(interval(ymd(as.character(max(rfm$cohort))),ymd(as.character(max(dates)))))
Formal class 'Interval' [package "lubridate"] with 3 slots
  ..@ .Data: num 2592000
  ..@ start: POSIXct[1:1], format: "2015-06-01"
  ..@ tzone: chr "UTC"
```

Seems like two things are different: we now have a time zone, UTC -- which stands for Universal Time, Coordinated, which is another name for the old-school GMT -- and the start point is in this date format, as opposed to the time stamp above. 

Now: do we really need a chained call to two functions, `ymd()` and `as.character()`? Wouldn't `as.POSIXct()` suffice? Let's try:

```
> as.period(interval(as.POSIXct(max(rfm$cohort), tz = 'GMT'),as.POSIXct(max(dates), tz = 'GMT')), months)
Error in while (any(start + est * per < end)) est[start + est * per <  : 
  missing value where TRUE/FALSE needed
```

Nope. But why? If you look up `?as.POSIXct` you will see that `tz =` is the argument that sets the time zone, so this should work. As it turns out, `lubridate` wants you to set the time zone for the interval, not separately for its ends. Like this:

```
> as.period(interval(as.POSIXct(max(rfm$cohort)),as.POSIXct(max(dates)), tz = 'GMT'), months)
[1] "1m 0d 0H 0M 0S"
```

I have no idea why this is so, and asking [StackOverflow](http://stackoverflow.com/questions/31999176/setting-the-posixct-time-zone-in-lubridateinterval) hasn't turned up anything so far.
