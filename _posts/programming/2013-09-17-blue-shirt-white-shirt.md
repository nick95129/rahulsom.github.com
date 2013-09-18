---
layout: post
title: "Blue Shirt, White Shirt"
description: "Why you should not trust your users."
category: 
tags: []
---
{% include JB/setup %}

![Blue Shirt White Shirt](https://upload.wikimedia.org/wikipedia/commons/thumb/1/10/Woman_in_white_Shirt_and_black_pants_and_Woman_in_blue_shirt_and_white_pants_jumping.jpg/800px-Woman_in_white_Shirt_and_black_pants_and_Woman_in_blue_shirt_and_white_pants_jumping.jpg)

Image from [Wikipedia](https://en.m.wikipedia.org/wiki/File:Woman_in_white_Shirt_and_black_pants_and_Woman_in_blue_shirt_and_white_pants_jumping.jpg). No! That is not Bob.

I call this the `Blue Shirt, White Shirt` problem. It goes like this:

Your customer, *Bob* calls you and says

----

*My car does not work.*

*It's actually quite simple, and I figured out what makes it stop. 
When I wear a blue shirt, it works just fine. But when I wear a white shirt,
it doesn't work.*

----

If you're a mechanic, you know there's some serious shit going on there, but
your customer has a hypothesis and you don't. Let's start looking into this problem
deeper.

----

*I woke up last morning and wore a blue shirt. Then I got to work.*

*Then I came home. The car was still working. You with me?*

*I had to go to my buddy's place. So I quickly changed to a white shirt, 
and tried starting my car. It just wouldn't.*

*So I said "let's go back to my blue shirt". So I went back in, and changed.
I got a call from Mom, and minutes later I start the car, and it works just
fine.*

*But I wanted to make sure I'm right so I also took my white shirt with me
to his place, and told him about this problem. He doubted me too.
So I put on the white shirt and tried starting it, and it wouldn't start.*

*Then we went out for dinner in his car, and I spilled some of that Thai Curry
on my white shirt. So I had to change back to my blue shirt, because, well anyways
my car wouldn't start with the white shirt.*

*Now I started the car, and came back home. But to be sure, I put on another white
shirt and tried starting the car. It just wouldn't.*

----

Now *Bob* has done an amazing analysis of the problem, and it's a great story.
The only problem with it is **it's all wrong**.

While having a diploma in car repair would help you trust my judgement of this analysis,
you probably know that any way.

A lot of customer problems in Software Development get reported this way. And by the 
time they get to the developer who needs to fix the problem, this is what it looks like

	CAR-1034: Make car start when customer is wearing white shirts.

	Severity: Blocker
	Type: Bug

What would have helped move things around is

	CAR-1034: Investigate intermittent starting trouble on Bob's car.

	Severity: Blocker
	Type: Support Task

The amount of time wasted by development and QA in reproducing Bob's "problem" is huge.
Perhaps it might have been simpler to find out why Bob's car does not start when the
engine is already warm.