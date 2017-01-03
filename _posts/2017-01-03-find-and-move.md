---
layout: post
title: Find and move in the Mac terminal
---

This works in Yosemite, BASH terminal. 

To find and list files in a folder that are older than n days -- say n = 15 -- do this:

```
find path/to/search/ -ctime +15 -type f -print0 | xargs -0 -n1
```

To also match some regex pattern do this, and notice the extended regex modifier `-E`:

```
find -E path/to/search -ctime +15 -regex ".*(leisure|plastic).*" -type f -print0 | xargs -0 -n1
```

To move these things to a different folder do this:

```
find path/from -ctime +15 -type f -print0 | xargs -0 -I '{}' mv "{}" path/to/
```

And that's all you need. You find these things with some StackOverflow and nixCraft rummaging, but none of this stuff ever seems to work on the first attempt. There will be missed quotation marks, or straight brackets where parentheses should be put instead, etc. The stuff above worked for me.
