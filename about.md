---
layout: page
title: About
---

For whatever reason, I've become increasingly more interested in software quality over the past few years. Maybe it's the perfectionist element- bugs bother me to the core, and the holy grail of "perfectly functioning software" is very enticing. Or it's the challenge of it- it's a puzzle of infamous difficulty, and I love the endorphin rush that comes at the end of a good puzzle. There's the human element too- a huge part of quality is determining whether or not the software is pleasing to its customers, and there's no greater joy than building something that people enjoy.

While these may explain my initial interest, there's a third element that keeps me hooked on the topic: its practical necessity. Today, teams of programmers are tasked with building and modifying applications of intimidating complexity week after week. Requirements change every day. Bugs are fixed as they arise. We've created innumerable tools focused on enabling software construction, but are we ending up with products that provide their advertised value to customers?

I think this puts us in the middle of a [second software crisis](https://en.wikipedia.org/wiki/Software_crisis)

I'm hoping to collect posts here that circle around this one central notion, to be able to consider different aspects of the problem in an exploratory environment, and to . If nothing else, it may be a useful resource for considering the many dimensions of software quality in one place.

# Values

- Stay practical
- Remember the customers
- Seek to understand

Throughout, I will try and keep the following values in mind to stay focused and write about what I think is interesting.

## Stay practical

One of my main goals is to always consider practical software when talking about quality techniques. A pet peeve of mine is when examples are too simple or focus on irrelevant or trivial areas of the application. My primary area of interest is interactive information systems, aka enterprise applications, aka Web 2.0, and probably many other names. I'm interested in applications where users create and maintain their own data via a rich user interface, be it web or native. To contrast, I'm not interested in advanced visual rendering techniques, video games, or infrastructure software such as operating systems or web browsers. I'm also not interested in batch processing, such as compilers or ETL processes. What I discuss here may apply to those, as I'm often interested in analyzing the essence of software itself, but everything that I think about is colored by the desire to build interactive information systems.

## Remember the customer

Software is meant to serve people. We get hung up a lot on the craft of software itself, but software is a means to an end. A perfectly functioning piece of software that doesn't meet its intended purpose is useless. For that reason, customer satisfaction has to be a cornerstone when thinking about quality. 

## Seek to understand

I'm not looking for quick fixes or band-aid procedures. It's a goal of mine to understand things. I won't shy away from theoretical concepts, but _Stay practical_ will keep me honest if I'm going to far down a path with no value. I just won't close the door esoteric or unconventional techniques or concepts. In order for a change to be made (and I believe a change should be made), we need to try different things. So I will engage in thought experiments, prototyping new designs, anything that ultimately results in a new bit of information about software reliability. That's the plan at least.

My current thinking is that the answer involves math, specifically logic. I think lightweight formal specification and model checking (with tools such as [Alloy](http://alloytools.org/) and [TLA+](https://lamport.azurewebsites.net/tla/tla.html) is a very compelling idea, though models have limitations in that they don't capture errors in construction. [Property-based testing](https://increment.com/testing/in-praise-of-property-based-testing/) is a variation on that theme, but can be used on actual code implementations, making it a useful complement to formal models. As I said, practicality is still a value of mine though, so I don't want the math to be too theoretical. A common antipattern I see is losing the forest for the trees when starting to bring math into the picture. The math needed to achieve software reliability seems much more like applied mathematics vs. pure mathematics, though we may drift into theory when it's relevant. We'll always tie it back to a practical goal though.

## A meta-note about values

_Stay practical_ and _Seek to understand_ are inherently in tension, but just as being too abstract and theoretical can have no practical utility, being overly practical can lead to being closed-minded and unwilling to try much more efficient things. I'm looking to find the holy grail of solutions to problems that are both robust and applicable to daily work.

Lastly, who am I? While I'm not hiding my identity, I don't think it's consequential. My experience, however, can provide some color to the ideas captured here. After all, my thoughts are a result of my experiences in the industry. I didn't do any substantial programming before college, though I was heavily into using and I started professionally programming in September of 2011. I've had a few different jobs since then.
