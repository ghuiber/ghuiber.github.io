---
layout: post
title: Updating the VGAM package on Yosemite
---

The [VGAM](http://cran.r-project.org/web/packages/VGAM/index.html) package is here. The usual `install.packages()` routine today pointed out, as it sometimes happen, that the numbers of the source and binary versions differ, with the source being newer, so it offered to compile the latter. If you accept, you need to have a Fortran compiler present. Below is how I went about it:

1. I got the latest stable GNU Fortran compiler (version 4.9 as of this writing) [here](http://hpc.sourceforge.net/) and installed it according to the instructions.
2. I set a symlink because VGAM wanted version 4.8: `sudo ln -s /usr/local/bin/gfortran /usr/local/bin/gfortran-4.8`.
3. I ran `install.packages("VGAM")` at the RStudio console.
