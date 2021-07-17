---
layout: layouts/post.njk
title: Rational function analysis
date: 2021-07-16T16:49:18.958Z
tags:
  - analysis
  - rational function
---
## Set-up

We consider the rational function
$$ f: x \mapsto \frac{ax^2 + bx + c}{dx + e}, \qquad x \in \mathbb{R}, x \neq - \frac{e}{d} $$

where $a,d \neq 0$.

Under what conditions is the range of $f$, $R_f = \mathbb{R}$? If $R_f \neq \mathbb{R}$, what is its range?

## Results
 
This question depends on the sign of the special quantity we will call the _special discriminant_ and denote by $s$.

$$ s = a(ae^2 - bde + cd^2) $$

If $s = 0$, then the function 'collapses' to a linear function.

If $s < 0$, then the function has range $R_f = \mathbb{R}$.

If $s > 0$, then $R_f = \left( -\infty, \frac{bd-2ae-2\sqrt{s}}{d^2} \right] \bigcup \left[ \frac{bd-2ae-2\sqrt{s}}{d^2}, \infty \right)$
