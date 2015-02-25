---
layout: post
title: Delayed macro substitution
categories:
- Stata
---
In Stata, you have local and global macros that can encapsulate all sorts of specific text: names of variables, constant values, even entire chunks of Stata code. Stata will interpret macros as soon as you invoke them, so that if you define

```
local thedata sysuse auto
local themodel regress mpg foreign weight
```

you can simply call

```
`thedata'
`themodel'
```

You can also chain or nest local macros. It makes for a little extra work and produces code that's a little harder to read, but using macros is the best way to ensure code consistency, so it's a good thing to get used to them.

One neat feature of macros is that you can delay their substitution with backslashes. This allows you to nest macros in a very specific way. Let's expand on the example above. You could have defined the macro `model' as a nested one:

```
local rhs foreign weight
local model regress mpg `rhs'
```

From your standpoint, the local `rhs' is nested inside the local `model'. Stata does not care, because as soon as you have it read "`rhs'", it substitutes "foreign weight". From its standpoint, this `model' is identical to what the previous definition would have generated: the string "regress mpg foreign weight". But now suppose that you need a little flexibility in what should go in the right-hand side of this regression equation: suppose that headroom might also matter to fuel economy, presumably because gains in headroom come at a cost in aerodynamics. You could do this:

```
local rhs1 foreign weight
local rhs2 foreign weight headroom
```

Then you could do

```
local model regress mpg `rhs1'
`model'
local model regress mpg `rhs2'
`model'
```

You have to redefine the local `model' twice because Stata substitutes the values of `rhs1' and `rhs2' as soon as you invoke them. There's a way around that. You could nest `rhs' into the definition of `model' with a delayed, as opposed to immediate substitution, using a backslash, and only change its content when needed:

```
local model regress mpg \`rhs'
local rhs foreign weight
`model'
local rhs foreign weight headroom
`model'
```

Delayed substitution is elegant. It lets you nest macros using their names as placeholders, and have Stata fill them in only when they are needed. Here's one final working demo that shows you how you can use delayed macro substitution in program definitions:

```
capture prog drop myDemo
program myDemo

syntax anything

local displaythis "Argument \`i' is \`addthis'"

local argct: list sizeof anything
forvalues i=1/`argct' {
   local addthis: word `i' of `anything'
   di "`displaythis'"
}

end

myDemo three blind mice
```

For more information, see [here](http://www.stata.com/support/faqs/lang/backslash.html).



