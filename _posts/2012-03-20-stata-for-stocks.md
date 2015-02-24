---
layout: post
title: Stata for stocks
categories:
- Stata
tags: []
---
The people at StataCorp are on Facebook, and the other day they linked to [this](http://aidwriting.wordpress.com/2012/03/16/checking-a-simple-stock-portfolio-with-stata-easily/) blog post by Paul Clist about checking on a stock you might own through clever use of the [stockquote](http://ideas.repec.org/c/boc/bocode/s456990.html) Stata command.

Last year I bought some Netflix stock when it fell to $77 after the Qwikster fail. I agreed with the general public that it was a stupid idea, but I still thought that the hit their stock took was a bit of an overreaction. The streaming business was still good. Maybe not $250 per share good, once the content suppliers would catch wise and raise their prices, but my family was still happy with it as a TV substitute. That's about the full extent of thought I'm going to ever put into picking any stock, so don't be too surprised that I don't make big bets. This one was just shy of $500 -- whatever round number of shares plus the broker's commission fit there.

Still, it was just never clear to me how good this choice of spending $500 was relative to the Nasdaq Composite. Of course, I could have looked it up on [Bigcharts](http://bigcharts.marketwatch.com/), but why not have a picture with the real dollars I have at stake on the y axis, and my true time line on the x axis? It wasn't too hard to expand Paul's code to a set of programs that can take any of the four stocks in my toy portfolio and put it against some appropriate stock market index, to show how it's been doing in one quick `tsline` graph. Here's Netflix as of last Friday:

{% include image.html url="/assets/2012/03/NFLX-300x218.png" %}

My code takes the starting dollar amount from my brokerage statement, and it augments both the holdings and the index baseline with any subsequent purchases or sales (there aren't any in this case). This way I can simply `collapse (sum)` both the "index" (transformed into a price-per-unit times the units held of each stock following Paul's formula) and the valuation of each stock into daily totals, and plot the performance of the whole portfolio relative to my stock index of choice, with actual dollars on the y axis. 

I like it. It works fine for me. I already use Stata to do the household's budget, I used it to compare true costs to own of water heaters when I was in the market for one, and I used it to track wet and dirty diapers when my kid was a few days old. So, thank you, Paul, for helping me find yet another civilian use for this fine piece of software. 
