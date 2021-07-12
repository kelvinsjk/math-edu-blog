---
layout: layouts/post.njk
title: "the story so far (part two: programming and coding)"
date: 2021-07-12T04:44:38.732Z
tags:
  - personal
---
I have always been interested in this area since a kid. GameFAQs was probably a big inspiration growing up: I wanted to create my own small website with some of its features. I remember borrowing huge books from the library. 

First I learnt HTML (which, after finally figuring out to use Notepad rather than Word, went decently well. And I've since grown to view this as a good transition for my interest in $\LaTeX$). 

Next I tried to tackle PHP. With my nonexistent knowledge of programming or how servers worked that didn't go so well. I'd read the book, roughly understand what's going on, but being unable to implement in on my local computer was a big obstacle to any progress.

School and life got busier and I kind of left this tech interest by the wayside. It took until university until I took the required MATLAB course as part of the chemical engineering curriculum. Learning the computing side of the language was really cool, but ultimately it was a tool more suited for specific computations.

As part of procrastinating for my FYP I took a MOOC (that was the start of the MOOC revolution) on Udacity learning Python. It was really awesome seeing the programming aspects I learnt in MATLAB translate to a more general purpose language, and it became a useful tool for me for the rare cases I wanted a computer to run through computations.

I think it was around the time I tried to implement MathJax on my WordPress blog that I got introduced to JavaScript. Now I can implement small programs on my blog/the browser that can be used by others! My first program is probably a version of [this inequality generator](https://adotb.xyz/randomly-generated-questions/inequalities-1/).

As a tutor/educator at a pre-university level, creating worksheets is often a case of modifying past questions slightly (either with new numbers or a slight twist in the logical reasoning), with truly creative questions a much rarer thing. This led me to try to implement these modifications with a computer incorporating randomly generating elements. I now think of some of my work as creating a "superset" of questions based off of past examinations.

Trying to implement JavaScript within WordPress was a bit of a chore so the next idea was to implement a mobile app, with increasing mobile adoption. I did try to look into it, and briefly tried to code a Flutter app but ultimately that was a step too far for my set of skills.

So this interest took a back seat for a while until the Covid-19 lockdown of 2020 provided me more time to give my math question generation project another whirl. Hybrid mobile app development seemed like a much more accessible route for me, where I can incorporate what I have already been doing with JavaScript and MathJax. Other than Onsen UI for UI elements and KaTeX (which worked a lot better for me than MathJax), I went about creating everything from scratch. [See a salvaged version of it here](http://mathbounty.adotb.xyz) 

I did most of my work on Monaca, which was a huge step up from using Notepad in the past. But discovering VSCode and its various tools supercharged what I'm able to do. I still had to learn many tricks (templating, DOM management, etc) and while I'm pretty proud with what I've achieved there it was simply not sustainable once lock down was lifted.

I was now better understanding web technologies, with the State of JavaScript Survey convincing me to try out TypeScript which I loved. My next plan was to now learn about one of the popular frameworks (React, Vue or Angular). Serendipitously I discovered Svelte and took to it immediately. Its approach to reactivity and DOM management was super natural and its excellent documentation meant it was super natural to pick up. 

With that came a series of projects ([an example](https:/1002.vercel.app)) I was able to produce was increasing efficiency (with a few of them just taking a few days to go from conception to the 'final state').

Next I was able to implement these questions along with ideas about having notes on the internet in a Sapper project [Math Atlas](https://math-atlas.vercel.app) which I'm still actively working on.

With a Covid-19 mini-lockdown in Singapore recently I also tried out SvelteKit and produced a [3D Vectors app](https://3d-vectors.vercel.app).

Through these projects I started how certain implementations of mine can probably be grouped up into a library: this is the genesis of the hsmath project. This blog will now give me a place to journal about some of my considerations as well as prototype some documentation. Here's to a fruitful journey along the way!