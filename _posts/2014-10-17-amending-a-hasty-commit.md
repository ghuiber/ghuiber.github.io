---
layout: post
title: Amending a hasty commit
tags:
- GitHub
- R
---
On my current project I occasionally have to report anomalies in input data to people upstream, who can look into them. I do this with html files which I knit from [R Markdown](http://rmarkdown.rstudio.com). They have to include prose, code, results, and pictures. As I edit them on my way to the final product, it's practical to cache some of the code chunks, especially if they require loading large input files or reading from a database.

The cache folders can get quite big. If it so happens that I haven't edited `.gitignore` to skip cache folders, my next commit will be slow and the Sync will fail because the enterprise GitHub server has a 50M file size limit.

But if you've already hit 'commit' and the Sync button in the GitHub app, what do you do? It turns out you can edit `.gitignore` so it knows better next time, maybe delete the offending files -- not that you strictly have to, but these cache folders won't be needed once the report is in its final format and they just take up disk space -- and then do this at the command line:

```
git status
git add --all
git commit --amend --no-edit
git status
```

The first `git status` will show that you have deleted files and modified `.gitignore`. The `git add --all` construct, unlike `git add .`, will stage the deletions as well as the modification. Then `git commit --amend --no-edit` is a brand new commit on top of the old one, as explained [here](https://www.atlassian.com/git/tutorials/rewriting-history/git-commit--amend/). The second `git status` confirms that all is well, which you can see again when you switch back to the GitHub app: the Sync button tells you that you're ahead by one commit, you click it, and the push is quick, because the huge cache files are gone.
