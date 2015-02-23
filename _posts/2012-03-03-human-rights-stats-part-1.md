---
layout: post
title: Human rights stats, part 1
---

I follow <a href="http://simplystatistics.tumblr.com/" title="Simply Statistics">@simplystats</a> on Twitter, and on March 1 they had a post that linked to an article in Foreign Policy about a guy who has <a href="http://www.foreignpolicy.com/articles/2012/02/27/the_body_counter?page=full">the coolest job in applied stats</a>. He works <a href="http://www.benetech.org/">here</a>.

The original piece described a quick algorithm that you can use to estimate the number of human rights violations using a technique first devised for counting fish in a pond. The gist of it is this: catch and release fish over two days. Tag the fish caught on the first day. Count each day's catch and the number of fish caught twice. That is the overlap. To estimate the number of fish in the pond, multiply the two days' catches and divide the total by the overlap.

I had a data set of insurance claims in Stata's memory at the time of my reading, with observations uniquely identified by a variable named claim_id.

I decided to use it as the model of a pond with as many fish in it as observations in my data set, so I wrote a little fishing program. It takes one argument: some round upper bound of the number of fish I might catch in a day. I'll call it <em>n</em>. It can be 100, or it can be 1,000. Here:

```
// try MSE

capture prog drop guessObservations

program guessObservations



args n // upper bound of a day's catch.



qui {

   local day1fishcount=int(runiform()*`n')

   local day2fishcount=int(runiform()*`n')

   

   forvalues i=1/2 {

      preserve

      tempfile day`i'fishlist

      sample `day`i'fishcount', count

      keep claim_id

      save "`day`i'fishlist'", replace

      restore

   }

   

   preserve

   drop _all

   use "`day1fishlist'"

   merge 1:1 claim_id using "`day2fishlist'"

   count if _merge==3

   local overlap=r(N)

   restore

   

   local totalfish=`day1fishcount'*`day2fishcount'

   if `overlap'>0 {

      local totalfish=`totalfish'/`overlap'

   }

   count

   local truect=r(N)

}



local fmt _col(30) %10.0fc

di ""

di "Fish caught on day 1:" `fmt' `day1fishcount'

di "Fish caught on day 2:" `fmt' `day2fishcount'   

di "Overlap:"              `fmt' `overlap'

di "Estimate:"             `fmt' `totalfish'

di "True count:"           `fmt' `truect'



end
```

My data set has some 150,000 observations. Choosing a small <em>n</em>, say <code>guessObservations 100</code>, sets me up for an overlap of zero, but even so the two catches multiplied together won't even come close to the true size of the population. This is a technique for counting hungry fish in a small pond, not in an ocean. The size of the daily catch should be representative of the total, so you can have some decent overlap.

Setting <em>n</em>=1,000 keeps it small enough relative to the total population that it's still possible to have zero overlap, but <em>n</em> is now large enough to overshoot wildly in that case. If I catch 900 fish each day with zero overlap, I will guess that there are 810,000 fish there. However, an overlap as small as 5 will get me pretty close to the true population.

Setting <em>n</em>=10,000 performs much better. I may still have a day when the fish won't bite, and get this:

```
. guessObservations 10000



Fish caught on day 1:                49

Fish caught on day 2:             4,182

Overlap:                              3

Estimate:                        68,306

True count:                     157,638
```

But with any luck, I will probably get this:

```
. guessObservations 10000



Fish caught on day 1:             9,662

Fish caught on day 2:             3,220

Overlap:                            220

Estimate:                       141,417

True count:                     157,638
```

The larger <em>n</em>, the larger the overlap, and the better the precision. That makes sense: in the limit, the true number times itself divided by itself will yield the true number.

But does <em>n</em> have to be very large relative to the size of the population? And does my guess -- or the uncertainty surrounding it -- depend on what probability distribution function I assume for the daily catch? Next time I'll be doing some simulations.
