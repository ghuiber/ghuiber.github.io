---
layout: post
title: Regularization and the Bible
---

There are two kinds of regression regularization: ridge, or L2, and Lasso, or L1. They both let you get rid of some variance (less is good) in return for picking up some bias (of which also less is good; that's the trade-off).

How they go about it makes sense if you compare two versions of the Ecclesiastes 7:16 verse, which is good advice either way you read it, but can be interpreted a little differently in the two flavors you'll see compared [here](https://www.biblegateway.com/passage/?search=Ecclesiastes+7%3A16-25&version=ESV;KJV).

The ESV could be read as "be righteous in all matters, but not overly so." That's L2, ridge. The KJV version might be read as "be righteous, but only about some things." That's L1, Lasso.

This all came up because I had to explain regularization recently, and because I just finished re-reading [Keep the Aspidistra Flying](http://en.wikipedia.org/wiki/Keep_the_Aspidistra_Flying). Ecclesiastes 7:16 makes an appearance toward the end of the book.
