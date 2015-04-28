---
layout: post
title: BTYD models
---

I had to do some customer churn modeling for work recently, and I used the [Pareto/NBD BTYD model](http://brucehardie.com/notes/009/pareto_nbd_derivations_2005-11-05.pdf), for which there is a handy [CRAN package](http://cran.r-project.org/web/packages/BTYD/index.html) thanks to some [good people at Wharton](http://news.wharton.upenn.edu/press-releases/2012/08/wharton-customer-analytics-initiative-announces-buy-til-you-die-btyd-models-package-release/).

Unfortunately, the Pareto-NBD log-likelihood function cannot be estimated as implemented in that package if you have customers with rich purchase histories -- in the hundreds of items -- which you do if you're a popular e-commerce platform that has been around for a while and it has saved its [RFM](http://en.wikipedia.org/wiki/RFM_%28customer_value%29) data diligently.

The reason for this is explained [here](https://github.com/theofilos/BTYD) and there is a fix too, based on a computational trick explained [here](https://hips.seas.harvard.edu/blog/2013/01/09/computing-log-sum-exp/). I am not sure why the author never suggested it as a formal patch to the original package. Maybe it's in the works. Until then, my patched version of the library is [here](https://github.com/ghuiber/BTYD2).

There's still a limitation. Though this fix allows you to estimate the four parameters of interest from a sample of customers that includes people with rich purchase histories, individual probabilities that such customers are still alive cannot be estimated: the `pnbd.PAlive()` function will return `NaN`.

This may not be a deal-breaker for you. There's room for a judgment call here, based on the increasing frequency paradox described in the [walkthrough](http://cran.r-project.org/web/packages/BTYD/vignettes/BTYD-walkthrough.pdf). Any marketing campaign should target newer or less engaged customers. For these, `pnbd.PAlive()` still works. Customers with rich histories who haven't been seen in a long time are probably dead, so there's no use worrying about them, while very active customers who have been seen very recently are best not bothered. 



