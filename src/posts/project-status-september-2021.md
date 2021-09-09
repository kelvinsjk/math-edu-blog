---
layout: layouts/post.njk
title: Project status - September 2021
date: 2021-09-09T16:02:14.133Z
tags:
  - commentary
---
## `math-edu`

### Features

As of September 10, 2021, `math-edu` is live on npm at version 0.08 with the following features
- `Fraction`s (with `gcd` and `factorize` functions)
- Algebra and typesetting: `Term`, `Expression`, `Polynomial` and `RationalFunction`
- `SquareRoot` and `NthRoot`
- 3D Vectors: `Vector`, `Line` and `Plane`
- `Complex` numbers
- Calculus
  - `Angle`s, `Exp` and `Ln` classes. `Trig` functions
  - `ExpFn`, `LnFn`, `PowerFn`, `SinFn` and `CosFn`: differentials, integrals (definite and indefinite)
  - Integration of special trigonometric functions like $\sin^2 x, \cos^2 x$, $\sin ax \cos bx$ etc
  - Integration by parts of integrals of the type $x^n f(x)$, where $f(x)$ is a logarithm, exponential, sine or cosine function.
- Random generators (integers, fractions, linear and quadratic polynomials, vectors)

Some features are more fully fleshed out then others, but this should provide a good base going forward. The main additions to this list will likely be more on differentiation and sequences and series.

## `math-edu-plus`

### Dependencies 

`math-edu-plus` takes on two dependencies:

- [simple-statistics](https://simplestatistics.org/) to handle most of the statistics features
- [math-erfinv](https://github.com/math-io/erfinv) for a more accurate `invNorm` function

As such, we will have to update the license to reflect them (ISC and Boost license) as compared to the vanilla `math-edu`, where we plan to use the MIT license.

### Features

The statistics features of the library will thus live in `math-edu-plus`. 

In addition, the classes and functions in the topic of functions (sets/intervals, domains and ranges, composite and inverse functions) are currently here as we are still uncertain if our implementation is the best way forward. With polish they could be migrated to `math-edu`.

There are also numerical methods implemented in `math-edu-plus`:

- `bisection` for root-finding
- `simpsons` for numerical integration
- `cramers` for solving linear equations (currently only 2x2 and 3x3 are supported)

We do not include these in `math-edu` as we think there are much better implementations of these features in other libraries. Our focus is largely on handling 'exact' mathematical quantities which we think is a gap in the current JavaScript open source landscape, while working with the default `number` type is probably much better using other libraries tailored to scientific computing. Nevertheless it is still in `math-edu-plus` so that my personal projects can avoid unnecessary dependencies.

The last feature of `math-edu-plus` currently is random generators of a more "exotic" nature as compared to what is offered in the vanilla `math-edu-plus`. At the moment we can generate a "nice" quadratic with leading coefficient between 1 and 8 and integer/rational roots, as well as a random vector that is perpendicular to another that has components with magnitude smaller than 9.

## Math Bounty showcase

The [Math Bounty](https://math-bounty.vercel.app) website/app is a showcase of the features of the library as we generate answers to old TYS questions as well as offer a superset of those questions with randomly generated elements. 

At the moment the 2007 papers have been fully implemented, which is what has guided the development of the libraries.

As an experiment, I tried solving/generating answers to the most recent 2020 papers. At the moment we can handle 8/21 questions with relative ease. These papers also suggests the statistics, vectors and integration features in the library are generally more robust compared to the others. We hope to bump the percentage of questions we are able to handle up to at least 11-12 in the next update.

As we develop the library further we expect to use a two-prong approach: use the library to solve questions from the newer papers to generate answers, and generate the question superset from the older ones.






