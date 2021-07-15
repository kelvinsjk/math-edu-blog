---
layout: layouts/post.njk
title: linear equation generator
date: 2021-07-15T11:38:33.677Z
tags:
  - documentation
  - Fraction
  - Term
  - TermArray
  - tutorial
---

> Fiddle with examples here at the [Linear Equation demo on Svelte REPL](https://svelte.dev/repl/303a5a74523a45d8a24b955a4d67ca44?version=3.38.3)

Mathematics is a visual language: just think about the stereotype of scientists/mathematicians and their chalk/white-boards (the author is especially
guilty). 

And even more so in math education; any additional mental effort to parse strings like `int 1/3 x^2 dx` makes learning the actual mathematical concepts
tougher; we want to show $\\displaystyle \int \frac{1}{3} x^2 \\, \mathrm{d}x$ in all its glory.

At the same time, we also want to harness the power of computing to help us generate and solve problems. 
A typical workflow may look like this:

1. Come up with a mathematical idea
2. Translate our mathematical ideas into the relevant data types of our programming language of choice
3. Write the necessary code to accomplish our goal
4. Translate our results from computing data back to a human readable form suitable for our target audience

In many of the author's projects, steps 2 and 4 often turn out to be overly complicated and 
take us away from the mathematical ideas we want to showcase. 

> The `math edu` project aims to smoothen the process from idea to output by making each step
> as close as possible to the underlying mathematical structure. 

In this post we will use the example of a linear equation generator to showcase how `math edu`
can accomplish this goal.

# Creating a linear equation generator

## Mathematical idea

We want to generate questions/solutions of equations of the type
$$ax+b = c,$$
where $a,b,c \\in \\mathbb{Q}$, $a \\neq 0$.

For illustration, let us first try to tackle the case where $a = \\frac{2}{3}, b=-3$ and $c=-\\frac{5}{7}$.

## The math edu approach

```typescript
/* the math edu approach */
// set up constants
const a = new Fraction(2,3);
const b = new Fraction(-3);
const c = new Fraction(-5,7);
// set up ax and (ax + b) expressions
const ax = new Term(a, 'x');
const ax_PLUS_b = new TermArray(ax, b);
// get question: latex output line 1
const question = `${ax_PLUS_b} = ${c}`;
// get answer: latex output line 2
const answer = `x = ${c.minus(b).divide(a)}`;
```

<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ \frac{2}{3} x - 3 = - \frac{5}{7} $ 
  </div>
  <div style="text-align: center;"> 
    $ x =  \frac{24}{7}$ 
  </div>
</div>

We think this approach is pretty idiomatic and quite closely aligned to the mathematical chain of thought. 

We first set up our constants $a,b$ and $c$ with the `Fraction` class. Then we create the $ax$ term by using the `Term` class.
An expression like $ax+b$ can be seen as an Array of terms (to be added) so we use the `TermArray` class.

With these setup, we can generate the $\LaTeX$ string for the question by using the `toString` method in those classes

> Every class in `math edu` comes with a `toString` method to convert to $\LaTeX$

The use of the template literal, along with
the naming convention makes the code to generate the question, `` `${ax_PLUS_b} = ${c}` ``, look and sound similar to the symbolic representation we are used to.

Finally, the use of chaining for our arithmetic functions `minus` and `divide` makes the code for the answer readable and compact.

## How we got here

The rest of this post will detail some of the challenges in implementing this project without
`math edu`, and showcase some of our features.

## Attempt 1

As we do not want to work with floats for this example, a first attempt at creating our linear equation generator could
be to generate six numbers (numerator and denominator for each of our constants $a,b$ and $c$.) For example, let $a = \frac{a_1}{a_2}$, etc.

```typescript
/* attempt 1 */
// set up constants
const a1 = 2, a2 = 3;
const b1 = -3, b2 = 1;
const c1 = -5, c2 = 7;
// get question: latex output line 1
const question1 = ` \\frac{${a1}}{${a2}} x + \\frac{${b1}}{${b2}} = \\frac{${c1}}{${c2}}`;
// generate answer
const answerNumerator = a2 * (c1 * b2 - b1 * c2);
const answerDenominator = a1 * b2 * c2;
// get answer: latex output line 2
const answer1 = `x = \\frac{${answerNumerator}}{${answerDenominator}}`;
```
<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ \frac{2}{3} x + \frac{-3}{1} = \frac{-5}{7} $ 
  </div>
  <div style="text-align: center;"> 
    $ x =  \frac{48}{14}$ 
  </div>
</div>

Immediately three problems catch my eye:

1. Fractions vs integers: as $b=-3$ is an integer, we will have to implement coding logic to typeset it correctly as $-3$ rather than as a fraction.
2. The derivation of our answer sure is complicated! While we can all derive that
`a2 * (c1 * b2 - b1 * c2)` and `a1 * b2 * c2` are the numerator and denominator of our final answer, it is rather unwieldy and not likely
something we want to do mentally often (vs the simpler `(c - b)/a`). Having to do such sums can take us out of our workflow unnecessarily.
3. The final answer $\frac{48}{14}$ needs to be simplified. We will have to implement coding logic to take common divisors.

### The Fraction class

The `Fraction` class provided by `math edu` solves these problem. Fractions are always simplified before being stored (with 'hoisting' of 
negative signs if necessary), and the `toString` method on the `Fraction` class handles the fraction vs integer cases.

The arithmetic functions are also implemented as class instance methods allowing us to get our answer with
`c.minus(b).divide(a)`.

See the [Fraction class tutorial](https://math-edu-blog.netlify.app/posts/the-fraction-class/) or API documentation (TODO: to be updated)

## Attempt 2

We now implement our linear equation generator project with the `Fraction` class. For illustration, we will also consider an alternate
attempt with constant $a=-1$.

```typescript
/* attempt 2a */
// set up constants
const a = new Fraction(2,3);
const b = new Fraction(-3);
const c = new Fraction(-5,7);
// get question: latex output line 1
const question2a = `${a}x + ${b} = ${c}`;
// get answer: latex output line 2
const answer2a = `x = ${c.minus(b).divide(a)}`;

/* attempt 2b */
const a2 = new Fraction(-1);
// get question: latex output line 3
const question2b = `${a2}x + ${b} = ${c}`;
```

<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ \frac{2}{3} x + - 3 = - \frac{5}{7} $ 
  </div>
  <div style="text-align: center;"> 
    $ x =  \frac{24}{7}$ 
  </div>
  <div style="text-align: center;"> 
    $ -1 x + - 3 = - \frac{5}{7} $ 
  </div>
</div>

We're getting closer. In fact the answer is exactly what we want. We do still have two problems:

1. In attempt 2b, the $-1 x$ can be better typeset to show $-x$. Our typical way of writing mathematics handles coefficients of 1, -1 and 0 differently
from other coefficients.
2. In attempt 2a, we have a $+ -3$. When adding a negative term we typically write it as just $-3$.

### The Term class

To handle the first problem, we introduce the `Term` class in `math edu`. A `Term` is made up of a `coeff` (coefficient) and `variable` so that is how we
have set up the constructor.

```typescript
/* Term class demo */
const twoThird = new Fraction(2,3);
// the term constructor
const x = new Term(1, 'x'); // latex output 1
const twoThirdX = new Term(twoThird, 'x'); // latex output 2
const negativeXSquared = new Term(-1, 'x^2'); // latex output 3
const zeroX = new Term(0, 'x'); // latex output 4
// a Fraction is a constant Term with empty string as the variable
const twoThirdTerm = new Term(twoThird); // latex output 5
// we can also convert Fraction to Term
const oneTerm = new Fraction(1).toTerm(); // latex output 6
```
<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ x, \quad \frac{2}{3}x, \quad - x^2, \quad 0, \quad \frac{2}{3}, \quad 1 $ 
  </div>
</div>

Notice how the `Term` class handles the 0, 1 and -1 coefficients when we have a variable, which will solve the problem we encountered in attempt 2b.

> A `Fraction` class can be thought of as a 'constant' `Term` and is converted as such through the `toTerm` method

### The TermArray class

A mathematical expression can be thought of as a sum of terms. It thus makes sense to represent an expression digitally as an `Array` of terms, which we implement
in `math edu` as the `TermArray` class.

The constructor takes `Term`s (and/or `Fraction`s and/or `number`s) as arguments and concatenate them into an array. The `toString` method then shows the representation of the
sum of these terms, typesetting any instance of $a + -b$ as we have encountered as $a - b$

```typescript
/* TermArray class demo */
const twoThird = new Fraction(2,3);
const twoThirdX = new Term(twoThird, 'x');
// the TermArray constructor
const twoThirdX_MINUS_three = new TermArray(twoThirdX, -3); // latex output 1
// automatic simplification demo
const x = new Term(1, 'x');
const oneQuarter = new Fraction(1, 4);
const x_MINUS_three_PLUS_twoThirdX_PLUS_oneQuarter = new TermArray(x, -3, twoThirdX, oneQuarter);
```
<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ \frac{2}{3} x - 3$
  </div>
  <div style="text-align: center;"> 
    $ \frac{5}{3} x - \frac{11}{4}$
  </div>
</div>

For the complicated `TermArray` we defined representing $x - 3 + \frac{2}{3} x + \frac{1}{4}$, our implementation has automatically combined
the 'like terms' to give the final `toString` output of $ \frac{5}{3} x - \frac{11}{4}$.

## Conclusion

The `Fraction`, `Term` and `TermArray` classes in `math edu` can handle the arithmetic and output conversion aspects of our project,
allowing us to focus on our key mathematical ideas.

## Supercharging our linear equation generator

But wait, there's more! 

With the earlier set-up, we manually picked our unknown constants $a,b$ and $c$. We can go even better: let's randomly generate these constants.
`math edu` provides a `getRandomFrac` function to do that.
```typescript
/* getRandomFrac */
const aRandom = getRandomFrac({avoid: [0]});
const [bRandom, cRandom] = [getRandomFrac(), getRandomFrac()];
```

Now we have a new question and answer every time! We can wrap it in a loop and use it to generate a worksheet. Or incorporate it into a web app
where we can generate a new question every time we press a button. 

We have implemented a version of such a functionality in the [Linear Equation demo on Svelte REPL](https://svelte.dev/repl/303a5a74523a45d8a24b955a4d67ca44?version=3.38.3).
Knowledge of [Svelte](https://svelte.dev) may be needed to understand this last section of the demo (we do love it so you may want to
take a look at it if you're creating web apps).

