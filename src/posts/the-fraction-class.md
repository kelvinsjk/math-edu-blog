---
layout: layouts/post.njk
title: the fraction class
date: 2021-07-14T11:38:33.677Z
tags:
  - class
  - documentation
  - Fraction
---

> Fiddle with examples here at the [Fraction demo on Svelte REPL](https://svelte.dev/repl/adec4e67e3664d2d9e76257c3b4b1c68?version=3.38.3)

The `Fraction` class is the main entry into the `math-edu` library. With real values comfortably handled by the JavaScript built in `Number`,
we will be living in the rational world in this library. 

In fact, almost all of high school style mathematics that requires 'exact' answers
can be thought of as a field extension of/vector space over the rationals, so this class will be foundational in this entire library.

## The `Fraction` class

The `Fraction` class has two properties: `num` for the numerator and `den` for the denominator.

```typescript
class Fraction {
  num: number // numerator: integer
  den: number // denominator: positive integer
}
```

This fraction is 'simplified' upon construction in the form $\frac{a}{b}$ such that the numerator $a$ is an integer
and the denominator $b$ is a positive integer such that $\gcd(a,b) = 1$.
The examples in the next section illustrates this simplification.


## The constructor

```typescript
constructor(num: number, den:number = 1)
/* examples */
const twoThird = new Fraction(2,3);             // {num: 2,  den: 3}
const negativeThree = new Fraction(-3);         // {num: -3, den: 1}
const zero = new Fraction(0, 6);                // {num: 0,  den: 1}
const negativeThreeFifth = new Fraction(6,-10); // {num: -3, den: 5}
const oneQuarter = new Fraction(0.25);          // {num: 1,  den: 4}
const errorFraction1 = new Fraction(1,0);       // ERROR: denominator cannot be 0
const errorFraction2 = new Fraction(1/3);       // ERROR: unable to convert float to Fraction
const errorFraction3 = new Fraction(2, 0.3);    // ERROR: non-integer denominator not supported
```

The most common construction is exemplified by `new Fraction(2,3)` where we provide both the numerator and denominator.

Integers are fractions so `new  Fraction(-3)` defaults to a denominator of `1`. 

`new Fraction(0, 6)` and `new Fraction(6, -10)` are examples where our numerator and
denominators are 'simplified'.

The constructor will attempt to convert non-integer inputs to a fraction, as in the case of `new Fraction(0.25)`.

Finally, common errors thrown will be because of a 0 or non-integer denominator as well as when we are unable to convert floats to the Fraction format due
to precision issues.

## .toString() conversion to LaTeX

The `toString()` method converts the fraction to a $\LaTeX$ string that can be typeset. 

We find the use of the template literal backtick particularly idiomatic
and will be using them extensively in our documentation.

```typescript
toString(): string
/* examples */
twoThird.toString()     // '\\frac{2}{3}`
`${negativeThree}`      // '- 3'
`${zero}`               // '0'
`${negativeThreeFifth}` // '- \\frac{3}{5}'
```

<div class="latex-blackboard">
  $\LaTeX$ output: <br>
  <div style="text-align: center;"> 
    $ \frac{2}{3}, \quad - 3, \quad 0, \quad - \frac{3}{5} $ 
  </div>
</div>

## Arithmetic Methods

The four arithmetic operations are implemented here as `plus`, `minus`, `times` and `divide`, where both `number` and `Fraction` types are allowed as input.
Exponentiation is also implemented as `pow`, taking a non-negative integer input.

Chaining is a useful technique to perform multiple operations in order.

```typescript
plus(f2: number|Fraction): Fraction
minus(f2: number|Fraction): Fraction
times(f2: number|Fraction): Fraction
divide(f2: number|Fraction): Fraction // f2 cannot be 0
pow(n: number): Fraction              // n must be a non-negative integer
/* examples */
twoThird.plus(-3)                                        // adding a number
const sum = twoThird.plus(oneQuarter)                    // adding a Fraction
twoThird.minus(oneQuarter)                               // subtraction
twoThird.times(negativeThreeFifth).divide(negativeThree) // chaining multiplication and division
negativeThreeFifth.pow(2)                                // exponentiation
/* LaTeX demo */
`${twoThird} + ${oneQuarter} = ${sum}`
/* More demos on the Svelte REPL at the top of the page */  
```

<div class="latex-blackboard">
  $\LaTeX$ demo: <br>
  <div style="text-align: center;"> 
    $ \frac{2}{3} + \frac{1}{4} = \frac{11}{12}$
  </div>
</div>

## Boolean Methods

```typescript
isInteger(): boolean
isEqual(f2: number|Fraction): boolean
/* examples */
negativeThree.isInteger() // true
twoThird.isInteger() // false
twoThird.isEqual(2) // false
twoThird.isEqual(twoThird) // true
twoThird.isEqual(new Fraction(-4, -6)) // true
```

## valueOf, toFixed, toPrecision

We convert our `Fraction` type back into the primitive `number` type through the use of the `valueOf` method.

The `toFixed` and `toPrecision` methods are also implemented on the Fraction class. They make use of the corresponding built-in methods after
conversion to the `number` type.

```typescript
valueOf(): number
toFixed(digits?: number): string
toFixed(precision?: number): string
/* examples */
oneQuarter.valueOf() // 0.25
twoThird.valueOf() // 0.6666666666666666
twoThird.toFixed() // 1
twoThird.toFixed(2) // 0.67
twoThird.toPrecision() // 0.6666666666666666
twoThird.toPrecision(3) // 0.667

```

## toTerm method

This will be covered in the section about the Term class.

## Static properties and methods

To be covered in a separate post