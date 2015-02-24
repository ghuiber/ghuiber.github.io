---
layout: post
title: Tidying up your R packages
tags:
- R
---
Do you have the same R packages installed in two places? Would you like to  remove the duplicates? You might find the script below useful:

{% highlight r %}
rm(list=ls(all=TRUE))

# define function to return duplicate packages and paths
tidyup <- function() {
   packs <- as.data.frame(installed.packages())
   paths <- levels(packs$LibPath)
   main <- subset(packs, LibPath==paths[2]) # base and recommended
   mine <- subset(packs, LibPath==paths[3]) # stuff I installed
   dups <- intersect(main$Package,mine$Package)
   return(list(paths,dups))
}   

# do the work:
cleanthis <- tidyup()
removethese <- cleanthis[[2]]          # here's the list of dups
fromhere <- cleanthis[[1]][3]          # I only want them on the main path
remove.packages(removethese, fromhere) # done

# check the result: 
# if length(tidyup()[[2]])=0, all is well. no dup packages left.
checkthis <- as.numeric(length(tidyup()[[2]]))
{% endhighlight %}

Why I wrote this:

A while back I chose to separate my package library over two file paths. One would be for base and recommended packages (1), the other for everything else (2). My notes on how I did that are [here]({% post_url 2012-11-07-setting-up-my-r-library-folder-on-a-mac %}), and my reasons are [here]({% post_url 2012-08-02-the-r-library-folder-on-a-mac %}).

Today, I wanted to update my [Zelig](http://projects.iq.harvard.edu/zelig). I used the wizard -- `source("http://r.iq.harvard.edu/zelig.installer.R")` as of this writing -- so I would get all the add-ons in one step. The wizard works under the assumptions that your library is all in one place. It installed a few packages that Zelig and its add-ons depend on on path (2), because it didn't find them there. They were present on path (1) though, so I ended up with duplicates. This is how I got rid of them.
