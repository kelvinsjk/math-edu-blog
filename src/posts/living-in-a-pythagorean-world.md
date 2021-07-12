---
layout: layouts/post.njk
title: living in a pythagorean world
date: 2021-07-12T11:17:17.998Z
tags:
  - commentary
---
Working within high school mathematics is often like living in a Pythagorean world. In particular, the Pythagorean belief that "everything is number".

Even when working with irrational quantities like $\pi, \sqrt{2}, e$ and $\ln 2$ we insist on the exact quantity/expression $\pi^2 - \frac{\sqrt{2}}{e} + \ln 2$ rather than the approximate decimal interpretation. 

I do agree with most aspects of this, though my engineering training has relaxed my stance somewhat, but it does mean a lot more work when trying to work with them digitally. And even more so in a JavaScript environment where all numbers are floats. 

The machine learning angle will probably grow to dominate (aside: I am very impressed by Wolfram Alpha and how good it is, especially how much it has improved compared to when I first found out about it, when it was already pretty good). That said, I still think the programmatic approach still has things to offer.

This is where I hope the hsmath library can do its little bit in that aspect by providing a framework to work with these mathematical quantities we often work with in high school. We stay in the integers (but can be converted back to floats at any point), and then do the necessary logic to get them to display nicely (the convention of 0s, 1s and negative numbers is another thing that caused me many headaches for a while, having to typeset a quantity like `-2a + -3b + 1c - 0d + 5e` as $-2a-3b+c+5e$.