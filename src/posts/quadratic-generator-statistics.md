---
layout: layouts/post.njk
title: Quadratic generator statistics
date: 2021-07-16T15:49:18.958Z
tags:
  - analysis
  - quadratics
  - inequalities
---
## Set-up

Suppose $a,b,c \in \mathbb{Z}$, $-9 \leq a,b,c \leq 9$ and $a \neq 0$, how are
the roots of the quadratic equation 
$$ ax^2 + bx + c = 0 $$
distributed (assuming a uniform distribution).

## Results
 
- | Rational roots | Irrational real roots | Complex roots
--- | :---: | :---: | :---:
Probability | 0.161 | 0.461 | 0.378

A quick analysis will also show that the discriminant ranges from -324 to 405, and there are a total of 6498 unique quadratic equations.

5538 of them are such that $\gcd(a,b,c) = 1$.
