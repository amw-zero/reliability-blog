---
layout: narrative
title: Why quality?
author: Alex Weisberger
---

First thing's first: why should we care about the quality of software? Does it matter?[^fn1] The answer, like all answers, is that it completely depends on the context. Software is vastly diverse, and not all applications require the same level of reliability. The most obvious litmus test is "will people die if the application fails to perform its job?" That isn't even the only dimension to consider, though. For example, people are very sensitive to mistakes made with their money even though someone is unlikely to lose their life if there's an error in a money transaction. Our time is precious, our money is earned, and we expect our banking software to be reliable.

Errors are themselves contextual. If a UI button is mistakenly released with the wrong color, it might not cause outrage in the customerbase or cost the company money. Color can be aesthetic, and while aesthetics are important, they're fairly easy to fix and generally don't cause harm if they're wrong. However, color can also convey semantic meaning. Consider a legend, where color can be used to distinguish between categories. If a navigation application mistakenly displayed the traffic as green instead of red, that could be extremely frustrating. Trusting the software legend could have gotten you stuck in a traffic jam. Within the same application, a failure to display the correct colors can be totally benign or very disappointing depending on the use case.

While I think a lot about correctness and reliability, there is more to quality than just that. In fact, those are just the prerequisites to quality. At a higher level, though, quality is about people, their feelings, and their expectations. When we use a software product, especially when we pay for it, we have an expectation of what we want in return. An expectation has an implicit context, e.g. we expect to repair a car at some point after purchasing it. Quality does not mean perfection- it means that our expectations are met.

This is the core of the question of quality - is the act of meeting expectations important with software?

## The Intuitive Argument

I'm always apprehensive to make fuzzy claims, but I think it's warranted here. This is just a value call on my part, and I understand that there are differing opinions.

My view is that meeting expectations is the foundation of commerce and business, and  Because of that, it's important for software to work properly.

This is doubly true if we're talking about paid software. I can't wrap my mind around it being acceptable to charge money for something that doesn't work. This is where software gets tricky, because a piece of software often performs so many functions that a large percentage of them do in fact work. However, it's pretty safe to say that some subset of functions in every piece of software simply does not work. This is just accepted in the industry, as evidenced by the existence of Zendesk. 

## A Lemon Market

> But the bad cars sell at the same price as good cars since it is impossible for a buyer to tell the difference between a good and a bad car; only the seller knows. [^fn2]

In the US, we have a word for a car that has all kinds of unexpected problems. We call it a "lemon," and there are laws that protect car buyers from purchasing them. The laws exist because of information asymmetry-- cars are complicated machines, and most people aren't car designers or mechanics. While the average buyer can do a test drive and spot obvious problems, they can't know about the faulty fuel-injection system that's going to break in 2 months. Again, expectations are contextual, and people know that cars will require maintenance at some point. Maybe we expect to replace tires every so often, and get regular oil changes, and replace an alternator after 5 years, and replace windshield wipers, etc. There are reasonable things to go wrong with a car, and our expectation forms a mental threshold. Once that threshold is crossed, we consider the car to be a poor investment, a waste of time, a waste of money, and a breach of trust.

Software is also complex. But unlike cars, there's almost nothing that you can do to software to inspect its quality from the outside. A car has a single primary function. Software usually has dozens, if not hundreds. As we know from manual testing, coming across bugs by "test driving" an application is not easy.



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

[^fn1]: I think definitely think it matters, but it's a little more complicated than that.

[^fn2]: [The Market for Lemons: Quality Uncertainty and the Market Mechanism](https://viterbi-web.usc.edu/~shaddin/cs590fa13/papers/AkerlofMarketforLemons.pdf), George A. Akerlof.

[^fn3]: [Pragmatic Software Testing](https://www.oreilly.com/library/view/pragmatic-software-testing/9780470127902/), Rex Black

