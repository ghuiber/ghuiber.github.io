---
layout: post
title: Macbook Pro running hot, draining battery after upgrading to SSD?
---

[Mine](http://support.apple.com/kb/sp619) did. That was an unpleasant surprise. Googling for a solution brought up untold amounts of speculation and wasted time.

What ended up working for me was resetting the System Management Controller (SMC), as documented [here](http://support.apple.com/kb/HT3964) and especially [here](http://www.reddit.com/r/apple/comments/1fadpa/quick_advice_for_anyone_planning_to_put_an_ssd_in/). You should see that Reddit comment thread, especially if you're also wondering whether you're supposed to enable TRIM.

Resetting the SMC brought down the [CPU core temperatures](http://www.bresink.com/osx/TemperatureMonitor.html) from about 90\\(^{\circ} \\)C to about 60\\(^{\circ}\\)C, low enough for the fan to not kick in. My Mac is once again as quiet as it used to be.
