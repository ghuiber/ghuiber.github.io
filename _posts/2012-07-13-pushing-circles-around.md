---
layout: post
title: Pushing circles around
tags:
- Stata
---
Occasionally, a need comes up for drawing a circle. Say you have a scattershot of points that follow a bivariate normal distribution, and you want to illustrate being within a given radius inside that scattershot somewhere. You will want to draw a circle so you can use its functional form to isolate the dots that fall inside it, as well as help guide your eye.

Nick Cox already explained how to draw a unit circle [here](http://www.stata-journal.com/sjpdf.html?articlenum=gr0010). The code below shows how you can start from there and draw any other circle.

```
clear all
set more off
pause on

// A circle starts like this:
// 1. unit circle: x^2+y^2=1
// 2. circle of radius r centered in origin: x^2+y^2=r^2
// 3. circle of radius r centered at (a, b): (x-a)^2+(y-b)^2=r^2

// OK, so 3. is the generic form. Use it to describe y:
// y-b= [+/-]sqrt(r^2-(x-a)^2)
// y  =b[+/-]sqrt(r^2-(x-a)^2)

// Now you can draw circles, one half at a time
// as Nick Cox explained it in the Stata Journal,
// using the twoway function graph:
// http://www.stata-journal.com/sjpdf.html?articlenum=gr0010

// If you want more than one circle in a picture,
// it's worth doing some setting up first.

// E.g., formula components with values to be
// filled in later can be written once here:

// 1. the square-root part
local sqrt    sqrt(\`r'^2-(x-\`a')^2)

// 2. functions for the top and bottom semicircles 
local topf    function y=\`b'+\`sqrt', range(\`rl' \`rr') \`c'
local bottomf function y=\`b'-\`sqrt', range(\`rl' \`rr') \`c'

// An example of a circle drawn in the 1st quadrant
local a=1
local b=1
local r=.5
local rl=`a'-`r' // x [r]ange [l]eft end
local rr=`a'+`r' // x [r]ange [r]ight end
local c color(red)
local circle1 (`topf' aspect(1)) (`bottomf')

// An example of a circle drawn in the 3rd quadrant
local a=-1
local b=-1
local r=.5
local rl=`a'-`r'
local rr=`a'+`r'
local c color(blue)
local circle2 (`topf') (`bottomf')

// Reference case: the unit circle
local a=0
local b=0
local r=1
local rl=`a'-`r'
local rr=`a'+`r'
local c color(black)
local circleu (`topf') (`bottomf')

// Might as well string them all together
local circles `circle1' `circle2' `circleu'

// Draw the picture
twoway `circles', legend(off)
```