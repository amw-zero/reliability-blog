---
layout: narrative
title: 'Input Space: the Final Frontier'
tags: test
author: Alex Weisberger
---

Input spaces can be astronomically large. Since they are also very countable, we'll see that this is not hyperbolic at all. The frightening implication of this is that in order for a function[^fn1] to be considered correct, it has to output the correct value for all possible inputs. This has been such an important concept in my thinking about reliability that it's worth giving it a name and reiterating it more precisely:

**The Axiom of Total Association:** _A function is considered correct if and only if each value in its domain is correctly associated with a value in its range._[^fn2]

Let's look at a function representation of the `ls` program. We'll ignore the countless arguments its accumulated over the years, and analyze a simplified version that only accepts a path to a directory and outputs a list of the filenames in that directory:

~~~
function ls(path: string): string[] { 
  ...
};
~~~
{: .language-typescript}


What is the input space of this function?

Here, `ls` takes in a single string. We immediately hit a wall: there are infinitely many strings. Of course, computers are physical machines, so we do have limits on the number of characters that can be in a string within a computer program. Let's represent the upper bound of the length of a string with _S_, and for simplicity let's assume that only lowercase English letters can be used. With these constraints, we can represent 26^_S_ individual directory paths which means it only takes _S_=7 to get us into the billions-of-possible-strings territory. We're on the hook for every single one.

Again, this is the crux of why reliability is so difficult. We program with strings constantly, they're a fundamental data type that we use in everyday software construction, and yet they

## Modeling Commercial Real Estate

Understanding the input space is crucial for testing something. If the input space is small, exhaustive testing (which is the ideal) becomes possible.

Category partition method.

Small scope hypothesis - limiting a problem to an upper bound. Show alloy data model.

DO-178C (avionics certification) + modifed condition / decision coverage


Certain concepts can shed light on a problem after they click. Before knowing them, the problem can be elusive and unattainable. For me, the concept of input space brought a lot of light to the problem of software reliability. 
I'm not alone in thinking that

At the beginning of a program, input data gets applied to a function. In an interactive application with a user interface, that data simply gets built up piecewise over time, and applied in increments. Even a program with no directly supplied inputs can retrieve input data, like when ls accesses the current directory for printing.

[^fn1]: We'll analyze functions because they're a fairly good proxy for programs in general. I recognize that programs consist of more than pure functions, but this doesn't change the utility of analyzing them. The concepts will still apply if we, for example, are verifying methods that perform side effects.

[^fn2]: Let's just start out by talking about functions where we know we can get an output value for each input value, known as [total functions](https://mathworld.wolfram.com/TotalFunction.html). Unfortunately the world can get less neat than that, but we'll cross that bridge when we get there. 