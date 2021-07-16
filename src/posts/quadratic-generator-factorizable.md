---
layout: layouts/post.njk
title: quadratic equation generator (factorizable)
date: 2021-07-15T11:38:33.677Z
tags:
  - documentation
  - PolynomialTerm
  - Polynomial
  - tutorial
---

> Fiddle with examples here at the [Quadratic Equation (factorizable) demo on Svelte REPL](https://svelte.dev/repl/88525574668d4ddeadba067531f2952b?version=3.38.3)

Now that we have come up with a [linear equation generator](https://math-edu-blog.netlify.app/posts/linear-equation-generator/),
the next natural step is to build a quadratic equation generator.

In this tutorial we will explore the ideas behind extending our `Term` and `Expression` classes we saw previously into
the `PolynomialTerm` and `Polynomial` classes.

> If you have an idea making use of a data structure that isn't implemented in `math edu`, we recommending extending the classes
> provided.

# Creating a quadratic equation generator (factorizable)

## Mathematical idea

We want to generate questions/solutions of equations of the type
$$Ax^2 +Bx + C = 0$$
where $A,B,C \\in \\mathbb{Z}$, $A \\neq 0$.

Since we want the simpler case where our quadratic is factorizable, we will have to factorize our expression into

$$(ax+b)(cx+d)=0$$
where $a,b,c,d \\in \\mathbb{Z}$, $a,c \\neq 0$.

Our answer will be $$ x = -\\frac{b}{a}, \\quad \\textrm{or} \\quad x = -\\frac{d}{c}$$

For illustration, let us first try to tackle the case where $a = 1, b = -2, c = 3, d = 4$.

## Attempt 1

```typescript
	/* attempt 1 */
	// set up constants
	const a = 1, b = -2, c = 3, d = 4;
	// set up ax, (ax + b), cx, (cx+d) expressions
	const ax = new Term(a, 'x');
	const ax_PLUS_b = new Expression(ax, b);
	const cx = new Term(c, 'x');
	const cx_PLUS_d = new Expression(cx, d);
	// get A,B,C and create the Ax2 + Bx + C expression
	const A = a*c;
	const B = a*d + b*c
	const C = b*d;
	const Ax2 = new Term(A, 'x^2');
	const Bx = new Term(B, 'x');
	const Ax2_PLUS_Bx_PLUS_c = new Expression(Ax2, Bx, C);
  // get ax2 + bx + c = 0 question: latex output line 1
	const question2 = `${Ax2_PLUS_Bx_PLUS_c} = 0`;
	// get factorized question: latex output line 2
	const question1 = `(${ax_PLUS_b})(${cx_PLUS_d}) = 0`;
	// get answers: latex output line 3
	const answer = `x = ${new Fraction(-b, a)} \\quad \\textrm{or} \\quad x = ${new Fraction(-d, c)}`;
	
```
<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ 3x^2 - 2x - 8 = 0$ 
  </div>
  <div style="text-align: center;"> 
    $ (x - 2)(3x + 4) = 0 $ 
  </div>
  <div style="text-align: center;"> 
    $ x = 2 \quad \textrm{or} \quad x = - \frac{4}{3}$ 
  </div>
</div>

Powered by the `Fraction`, `Term` and `Expression` classes I would say this first attempt isn't too bad already. Our output is what we want, and the
only major downer in setting it up was having to figure out $A = ac$, $B = ad + bc$ and $C = bd$ from ${Ax^2 + Bx + C = (ax+b)(cx+d)}$.

Polynomial expansions like this is something we encounter rather regularly so let's add some functionality to our codebase.

> Almost all additional classes in `math edu` are extended from the `Fraction`, `Term` and `Expression` classes

## How we built the PolynomialTerm and Polynomial classes

This section details some of the ideas in building the `PolynomialTerm` and `Polynomial` classes in `math edu`.
The hope is that you will be able to use some of these ideas to build your own customized data type if necessary.
Skip to the next section if you just want to see it being used.

The JavaScript `extends` keyword help us create new classes that still retain the functionality of the old. In particular, when
working with polynomials, we still want to reuse the `toString` methods of the `Term` and `Expression` classes to handle 
typesetting of coefficients and plus/minus signs.

While scalar multiplication of a term is pretty well defined (and is implemented as `multiply`), multiplication of terms 
become more ambiguous. If we multiply the terms $5x$ and $2 \\sin x$, we may expect to get $10 x \\sin x$ (an experimental version
of this is provided in the `multiply` method). 

Polynomials behave slightly differently: when we multiply $5 x$ and $2 x^2$,
$10 x x^2$ isn't a 'nice' final result: we want to get $10 x^3$.

To get our code to understand the this special version of multiplication, it will need to understand that a variable like $x^2$ is made up of
the power $2$ and a "`variableAtom`" $x$. This observation is how we start extending `Term` into a new class, the `PolynomialTerm`.

```typescript
class PolynomialTerm extends Term{
  variableAtom: string
  n: number; // the power
... 'code for constructor and additional methods'
```

```typescript
constructor(coeff: number|Fraction, variableAtom = 'x', n: number){
  // create variable from variableAtom and n: a simplified version is shown here
  const variable = `${variableAtom}^${n}`;
  // calls the Term constructor
  super(coeff, variable);
  // stores the new information unique to Polynomial Term
  this.variableAtom = variableAtom;
  this.n = n;
}
```

We can now implement the multiplication of `PolynomialTerm`s:
```typescript
multiply(term2: PolynomialTerm){
  // exponent rule
  const newN = this.n + term2.n;
  // multiply coefficients
  const newCoeff = this.coeff.times(term2.coeff);
  // returns new Polynomial Term
  return new PolynomialTerm(newCoeff, this.variableAtom, newN);
}
```

Just like how an `Expression` is an array of `Term`s (which we will sum),
a `Polynomial` extends `Expression` by being an array of `PolynomialTerm`s.

A `Polynomial` can be created by calling the constructor with an array of `PolynomialTerms`, or
through a coefficient array (the syntax is slightly
different from the `Expression` constructor because of the need for these two cases). We will be using the latter later.

It comes built-in with a `multiply` method that multiplies `Polynomial`s (built off of the corresponding method for the `PolynomialTerm`).

Refer to the documentation for more details on working with them. Now back to our quadratic generator.

## Attempt 2

```typescript
/* attempt 2 */
// set up constants
const a = 1, b = -2, c = 3, d = 4;
// set up (ax + b), (cx+d) expressions
const ax_PLUS_b = new Polynomial([a, b], {initialDegree: 1, ascendingOrder: false});
const cx_PLUS_d = new Polynomial([c, d], {initialDegree: 1, ascendingOrder: false});
// create the Ax2 + Bx + C expression
const Ax2_PLUS_Bx_PLUS_c = ax_PLUS_b.multiply(cx_PLUS_d);
// get factorized question: latex output line 1
const question1 = `(${ax_PLUS_b})(${cx_PLUS_d}) = 0`;
// get answers: latex output line 2
const answer = `x = ${new Fraction(-b, a)} \\quad \\textrm{or} \\quad x = ${new Fraction(-d, c)}`;
// get ax2 + bx + c = 0 question: latex output line 3
const question2 = `${Ax2_PLUS_Bx_PLUS_c} = 0`;
```

`math edu` takes care of the polynomial expansion for us: no more $B = ad + bc$ calculations.


## Supercharging our quadratic equation generator

Similar to what we did for our linear equation generator, we can now bring in random elements and create a web app.

We have also built in a `toFactor` method for our `Fraction` class, returning a (linear) `Polynomial` to make things even faster.

```typescript
/* supercharged quadratic generator */
// generate necessary items
const root1 = getRandomFrac();
const factor1 = root1.toFactor();
const root2 = getRandomFrac();
const factor2 = root1.toFactor();
const quadratic = factor1.multiply(factor2);
// get output
const question1 = `(${factor1}) (${factor2}) = 0`;
const answer = `x = ${root1} \\quad \\textrm{or} \\quad x = ${root2}`;
const question2 = `${quadratic} = 0`;
```

We have implemented a version of such a functionality in the [Quadratic Equation (factorizable) demo on Svelte REPL](https://svelte.dev/repl/88525574668d4ddeadba067531f2952b?version=3.38.3).
Knowledge of [Svelte](https://svelte.dev) may be needed to understand the web app aspect of the code (we do love it so you may want to
take a look at it if you're creating web apps yourself).

