---
layout: layouts/post.njk
title: gcd function
date: 2021-07-12T11:38:33.677Z
tags:
  - function
  - gcd
  - arithmetic-methods
---
Before embarking on implementing the `Fraction` class the `gcd` function is the first function we will implement for the hsmath library to simplify our fractions to "simplest form".

## How it works

We take care of cases with 0 first. $\texttt{gcd}(0,0)$ throws an error while $\texttt{gcd}(0,a)=\texttt{gcd}(a,0)=a$ for all non-negative integers $a$

For all other cases we will use the standard Euclidean algorithm. 

I've probably copied this type of code countless times from a lot of Google searches and question-and-answer sites/forums before the current implementation: so thank you to all who have contributed to those before.

Remark: we will throw an error for non-integer inputs as well.

## Considerations

We will always return a positive number as the result. It was tempting to return a negative gcd for cases where both inputs are negative (it makes may potentially make handling negative signs in fractions easier) but decided against.

## Extended GCD

Trying to generate the set of vectors perpendicular to a given vector sent me down a rabbit hole re-learning about Diophantine equations and seeing how the extended Euclidean algorithm can be of use there. There was a brief consideration of extending this function, but at the moment I think writing out a separate function for that use case might be more prudent.