---
layout: post
title: "Functional Programming in Scala"
description: ""
category: 
tags: []
---
{% include JB/setup %}
A couple of months back I took the 
[Functional Programming Principles in Scala](https://class.coursera.org/progfun-2012-001/class/index) 
course at Coursera. It's a beautiful course offered by EPFL, specifically 
[Martin Odersky](https://twitter.com/odersky).

Personally I've been working on Java for several years, and at work use Groovy and Grails. At one point
I have used scala in small side projects at work. And all these things have impacted one another. Things
I learn in one language change the way I write code in another language. However there is the imperative
programmers burden. As a programmer you've been taught by C, C++ and then Java to think in terms of steps.

Learning Groovy as part of an offering from SpringSource did bring in several closures that let you write 
*slightly more functional* code. There were the closures you could pass to functions in collections and
get cleaner code, i.e. it was easier to write functional code. However it was just as easy to write bad
functional code or imperative code.

Scala, for one makes it incredibly hard to use mutable lists. Which means the likelyhood of a developer
being tempted to write bad functional code goes down significantly. Immutability is the defalt behavior
in Scala. So much so that you have a `val` declaration in addition to the `var` declaration to make sure
you think twice before writing mutable code. And this has changed how I think of my code back in Groovy
and Java.

If you're a Java programmer, whether or not you plan to use Scala in one of your projects, I recommend
you take this course. If for no other reason, then just because *it's free*.