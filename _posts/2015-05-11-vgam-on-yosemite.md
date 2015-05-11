---
layout: post
title: Updating the VGAM package on Yosemite
---

The usual `install.packages()` routine for the [VGAM](http://cran.r-project.org/web/packages/VGAM/index.html) package today pointed out that the numbers of the source and binary versions differed and the source was newer, so it offered to compile it. If it hppens to you and you accept, you must have a Fortran compiler present. Below is how I went about it:

1. I got the latest stable GNU Fortran compiler (version 4.9 as of this writing) [here](http://hpc.sourceforge.net/) and installed it according to the instructions.
2. I set a symlink because VGAM wanted version 4.8: `sudo ln -s /usr/local/bin/gfortran /usr/local/bin/gfortran-4.8`.
3. I ran `install.packages("VGAM")` at the RStudio console.
