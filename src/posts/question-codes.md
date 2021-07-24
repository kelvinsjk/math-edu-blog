---
layout: layouts/post.njk
title: question codes
date: 2021-07-22T04:55:25.203Z
tags:
  - function
  - documentation
  - question-code
  - math-edu-plus
---
## Introducing `math-edu-plus`

The `math-edu` library is designed to be a lightweight library containing many of the common mathematical constructions and functions we encounter in mathematics education, with implementations like the `Fraction`, `Term` and `Polynomial` classes.

In `math-edu-plus`, I collect some of my less used implementations that may be only relevant in a few special cases as I create content for my projects. It could also used as a testing ground for newer features: if a new feature first implemented in `math-edu-plus` sees repeated use, it may be a good candidate to move it to the main `math-edu` library.

## Question codes

Personally, the main use case of the `math-edu` library is to generate math problems and their corresponding solutions. Due to the random nature of this process, and how that is almost always done on the front-end, we do not have a record of what was generated.

It is useful to be able to keep track of the specific question that was generated for a few reasons:
- bug fixing
- for an end-user to return to a particular question
- for the developer/creator to generate a specific question for pedagogy purposes

Until we implement a back-end solution to this (unlikely in the near future given my set of skills and other things I also want to do), one way to do this is to generate "question codes" for each specific question.

In my initial attempts of this before creating `math-edu`, I created what I termed the "UQN" (unique question number) where each specific implementation of a question was associated with a unique number.

Relying on numbers made the final code quite readable, and it is easier implementing functions to check the validity of UQNs provided. But the fact that there are only 10 digits made it quite verbose.

Encoding one digit positive integers into a UQN was straightforward, but even something simple like a negative integer needed two digits to encode. To prevent verbosity I often tried to encode the signs of three different numbers into 1 digit of the UQN ($2^3 = 8$). 

This made creating the UQN and decoding the UQN to the actual variables used to creating the question and answer a bit cumbersome.

## Encoding and decoding in `math-edu-plus`

In `math-edu-plus` I've decided to implement functions for encoding and decoding as well. This time round, however, I will be attempting to encode to an ASCII string. With more characters to work with, almost every variable choice can be encoded to one digit.

In a first attempt (subject to change as I explore this more), I have decided to encode `0`-`9`, `a`-`z`, `>` and `<` as themselves. Negative numbers -1 to -9 will likely be encoded to `A`-`J` (avoiding `I` due to potential ambiguity). $\leq$ and $\geq$ will be encoded to `[` and `]`. In the future, functions could probably be encoded to the lower case alphabets. 

It still has the problem of verbosity as in the number implementation, and now the generated question code is less readable, but I hope such an implementation will make it easier to encode and parse/decode.

In the future, this question code string could also be further compressed as we are unlikely to use the whole ASCII set. Such encoding and decoding functions should be able to be implemented on top of this first version without any changes. 