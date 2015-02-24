---
layout: post
title: MathJax in your GitHub blog
tags:
- Jekyll
---

I'm patching up a few small dings and cracks that happened while moving my Wordpress blog over here.

That move happened following directions posted [here*](http://blog.8thcolor.com/en/2014/05/migrate-from-wordpress/) and it probably would have been completely uneventful, had my blog entries been strictly prose and hyperlinks. But they have some code blocks -- some of them in Stata, for which there is no Pygments lexer -- and some LaTeX, which is not supported by default.

You fix the LaTeX bit by turning on MathJax support. I followed Christopher Poole's [instructions](http://christopherpoole.github.io/using-mathjax-on-github-pages/) with two small tweaks:

1. You can put that piece of Javascript in a `<head>` section that you stick on top of the page where you'll be using math, because you can mix html with Markdown in a pinch. I just put it in `_includes/head.html`, so it would be available everywhere.
2. I didn't change my markdown from `redcarpet` to `kramdown` in my `_config.yml` file. I mean I did at first, but I switched back because `kramdown` mangled all my code blocks.

____
\* I didn't use [the official Wordpress importer](http://import.jekyllrb.com/docs/wordpress/) because I liked how these [8thcolor.com](8thcolor.com) people made use of a Wordpress-exported xml file.