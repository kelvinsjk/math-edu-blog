---
layout: layouts/post.njk
title: quadratic equation generator (irrational roots)
date: 2021-07-26T11:38:33.677Z
tags:
  - documentation
  - SquareRoot
  - Polynomial
  - tutorial
---

> Fiddle with examples here at the [Quadratic Equation (irrational roots) demo on Svelte REPL](https://svelte.dev/repl/dfdd250019b94276b9e2112cc8096ccd?version=3.40.3)

Now that we have come up with a [quadratic equation generator (factorizable)](https://math-edu-blog.netlify.app/posts/quadratic-generator-factorizable/),
the next natural step is to allow for irrational roots.

The `SquareRoot` class will thus be the main focus of this tutorial, in addition to the `Polynomial` class we have seen previously.

# The `SquareRoot` class

Built off of the `Term` class, the `SquareRoot` class models surds/radicals of the form $a \sqrt{b}$, where $a$ is our "coefficient" and 
$\sqrt{b}$ is the "variable". We will also be referring to $b$ as the `radicand`.

> More specifically, the `SquareRoot` class extends the `NthRoot` class that extends the `Term` class.

The quality of life features built into this class includes:

- extracting perfect squares from $b$ such that the `radicand` will be square-free. (e.g. $\sqrt{24}$ will be converted to $2\sqrt{6}$. This works for prime factors up until 97. 
- "rationalizing" denominators such that the `radicand` will be an integer. (e.g. $\sqrt{\\frac{3}{2}}$ will be converted to $\frac{1}{2} \sqrt{6}$)

```typescript
/* SquareRoot class example */
const rootTwo = new SquareRoot(2);
const twoRootThree = new SquareRoot(3, 2);
const root24 = new SquareRoot(24);
const rootThreeOverTwo = new SquareRoot(new Fraction(3,2));
// LaTeX output:
`${rootTwo}, ${twoRootThree}, ${root24}, ${rootThreeOverTwo}`;
```

<div class="latex-blackboard">
  <div class="blackboard-heading">$\LaTeX$ output:</div>
  <div style="text-align: center;"> 
    $ \sqrt{2}, \quad 2 \sqrt{3}, \quad 2\sqrt{6}, \quad \frac{1}{2} \sqrt{6} $
  </div>
</div>


# Creating a quadratic equation generator (irrational roots)

## Mathematical idea

We want to generate questions/solutions of equations of the type
$$ax^2 +bx + c = 0$$
where $a,b,c \\in \\mathbb{Z}$, $a \\neq 0$.

The roots will be based on the quadratic formula:

$$\displaystyle x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

For illustration, let us first try to tackle the case where $a = 1, b = -2, c = -4$.

## Implementation

The `SquareRoot` class will take care of the radical/surd simplification and we will use the `Expression` class to handle the addition $-b + \sqrt{b^2-4ac}$.
The negative case can be handled by letting the coefficient of the radical term be $-1$.

For potential simplification of the fraction (the denominator of $2a$), we have decided split up our answer expression into two terms,
$-\frac{b}{2a}$ and $\frac{1}{2a} \sqrt{b^2-4ac}$.

```typescript
/* specific implementation */
// set up constants
const a = 1, b = -2, c = -4;
// set up quadratic equation
const quadratic = new Polynomial([a, b, c]);
// calculate roots
const discriminant = b*b - 4*a*c;
const minus_b_over_2a = new Fraction(-b, 2*a);
const positiveRadical = new SquareRoot(discriminant, new Fraction(1,2*a));
const negativeRadical = new SquareRoot(discriminant, new Fraction(-1,2*a));	
const root1 = new Expression(minus_b_over_2a, negativeRadical);
const root2 = new Expression(minus_b_over_2a, positiveRadical);
// LaTeX output: line 1
const qn1 = `${quadratic}=0`;
// LaTeX output: line 2
const ans1 = `x = ${root1} \\textrm{ or } x = ${root2}`;
```
<div class="latex-blackboard">
  <div class="blackboard-heading">$\LaTeX$ output:</div>
  <div style="text-align: center;"> 
    $ x^2 - 2x - 4 = 0$ 
  </div>
  <div style="text-align: center;"> 
    $ \displaystyle x = 1 - \sqrt{5} \textrm{ or } x = 1 + \sqrt{5} $ 
  </div>
</div>

This set up handles all the minute details (simplification of our radicals and fractions) such that we do not need to modify our code for rational roots: try
setting up with `a=1, b=-2, c=-3` and we will get $x=-1 \textrm{ or } x = 3$ without having to change any single line of code: pretty neat!


## Randomly generated quadratic equations

We can now modify our code to randomly generate our question and answers. Since our code can handle both rational and irrational roots, we just have to ensure that
our discriminant $b^2-4ac$ is non-negative to proceed.

We will use the `getRandomInt` function in the library to do so.


```typescript
/* random implementation */
// generate necessary items: ensuring there are real roots
let a = getRandomInt(1,9), b = getRandomInt(-9,9), c=getRandomInt(-9,9);
let discriminant = b*b - 4*a*c;
while (discriminant < 0){
  a = getRandomInt(1,9); b = getRandomInt(-9,9); c = getRandomInt(-9,9);
	discriminant = b*b-4*a*c;
}
// change to simplest form;
const divisor = gcd(a,gcd(b,c));
a = a/divisor; b = b/divisor; c = c/divisor;
discriminant = b*b-4*a*c;
// set up quadratic equation
const quadratic = new Polynomial([a, b, c]);
// calculate roots
const minus_b_over_2a = new Fraction(-b, 2*a);
const positiveRadical = new SquareRoot(discriminant, new Fraction(1,2*a));
const negativeRadical = new SquareRoot(discriminant, new Fraction(-1,2*a));	
const root1 = new Expression(minus_b_over_2a, negativeRadical);
const root2 = new Expression(minus_b_over_2a, positiveRadical);
// LaTeX output: line 1
const qn1 = `${quadratic} = 0`;
// LaTeX output: line 2
const ans1 = `x = ${root1} \\textrm{ or } x = ${root2}`;
return [qn1, ans1]
```

We have implemented a version of such a functionality in the [Quadratic Equation (irrational roots) demo on Svelte REPL](https://svelte.dev/repl/dfdd250019b94276b9e2112cc8096ccd?version=3.40.3). Knowledge of [Svelte](https://svelte.dev) may be needed to understand the web app aspect of the code (we do love it so you may want to take a look at it if you're creating web apps yourself).

