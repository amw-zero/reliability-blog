---
layout: narrative
title: Why reliability?
author: Alex Weisberger
---

First thing's first: why should we care about the reliability of software? Does it matter?[^fn1] The answer, like all answers, is that it completely depends on the context. Software is vastly diverse, and not all applications require the same level of reliability. The most obvious litmus test is "will people die if the application fails to perform its job?" but that isn't the only dimension to consider. For example, while human life is not at stake in a typical money transaction, people are very sensitive to mistakes made with their money. If my credit card gets declined because my bank added an extra zero to a previous charge, I'm going to immediately be worried that I'll never get it back. No matter what, it will take time to resolve. Our time is precious, our money is earned, and we expect our banking software to be reliable.

Error classes are themselves contextual. If a UI button is mistakenly released with the wrong color, will that cause outrage in the customerbase or cost the company money? Probably not. Color can be aesthetic, and while aesthetics are important, they're fairly easy to fix and generally don't cause harm if they're wrong. However, color can also convey meaning. Consider a legend, where color can be used to distinguish between categories. If a navigation application mistakenly displayed the traffic as green instead of red, that could be extremely frustrating. I trusted the legend, and it fed me to the wolf of a traffic jam. Within the same application, a failure to display the correct colors can be totally benign or very disappointing depending on the scenario.

There's a theme here. Reliability is about people, their feelings, and their expectations. When we use a software product, especially when we pay for it, we have an expectation of what we want in return. An expectation has an implicit context, e.g. we expect to repair a car at some point after purchasing it. Reliability does not mean perfection- it means that our expectations are met.

(stronger) We should care about reliability, because selling someone something that doesn't meet their expectations is bad for everyone

## A Lemon Market

> But the bad cars sell at the same price as good cars since it is impossible for a buyer to tell the difference between a good and a bad car;
only the seller knows. [^fn2]

In the US, we have a word for a car that has all kinds of unexpected problems. We call it a "lemon," and there are laws that protect car buyers from purchasing them. 



I don't work on avionics or banking software, but I have worked on a navigation app before. I care about reliability when a failure will cost a lot of money, cause customers to leave, cause parts of an application to be unusable, leak sensitive data, etc. 

There's 

The need for reliability can be viewed as a predicate that 

When \(a \ne 0\), there are two solutions to \(ax^2 + bx + c = 0\) and they are
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

This risk context has to remain at the forefront of any discussion about software reliability, because in the absence of perfect software,  reliability is a budgeting problem. This makes the question of "why reliability?" in the global sense a red herring. At the highest level, the better question is: "what do we want to be reliable?"

> Test where the odds favor the highest benefit. Finding likely bugs is beneficial. Finding dangerous bugs is beneficial. The more likely or dangerous any particular group of potential bugs is, the more time and money you should invest in looking for them. Quality risk analysis makes testing a form of risk management. [^fn3]

I particularly like this pragmatic view of software verification, which puts project context at the root of the testing strategy. Instead of thinking of testing as way to prove that every miniscule aspect of a system works under all possible circumstances, we shift to talking about testing as a way of hedging against the riskiest parts of *our* project. Notably, this is an admission that we are simply unable to easily produce the holy grail of error-free software. If we espouse this view, we accept that errors will happen and instead focus on identifying and minimizing the most relevant potential errors to us.

## When we do want reliability, why?

What makes us identify something as requiring reliabilty? This can't be related to technology- technology doesn't feel or need. 

Start by asking your customers directly, or at least by talking to people on the business side.

[^fn1]: Well, I definitely think it matters. But, it's a little more complicated than that.
[^fn2]: [The Market for Lemons: Quality Uncertainty and the Market Mechanism](https://viterbi-web.usc.edu/~shaddin/cs590fa13/papers/AkerlofMarketforLemons.pdf), George A. Akerlof.
[^fn3]: [Pragmatic Software Testing](https://www.oreilly.com/library/view/pragmatic-software-testing/9780470127902/), Rex Black

