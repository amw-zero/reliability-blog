---
layout: narrative
title: Why reliability?
author: Alex Weisberger
---

First thing's first: why should we care about the reliability of software? The answer, like all answers, is that it completely depends on the context. Software is vastly diverse, and not all applications care equally about reliability. The most obvious litmus test is: "will people die if the application fails to perform its job?" but that isn't the only dimension at play. While human life is not at stake in a typical money transaction, people generally don't appreciate mistakes being made with their money. An understanding customer may tolerate the first double-charge to their account, but they may cancel their subscription after the second time.

> Test where the odds favor the highest benefit. Finding likely bugs is beneficial. Finding dangerous bugs is beneficial. The more likely or dangerous any particular group of potential bugs is, the more time and money you should invest in looking for them. Quality risk analysis makes testing a form of risk management. [^fn1]

I particularly like this pragmatic view of software verification, which puts project context at the root of the testing strategy. Instead of thinking of testing as way to prove that every miniscule aspect of a system works under all possible circumstances, we shift to talking about testing as a way of hedging against the riskiest parts of *our* project. Notably, this is an admission that we are simply unable to easily produce the holy grail of error-free software. If we espouse this view, we accept that errors will happen and instead focus on identifying and minimizing the most relevant potential errors to us.

This risk context has to remain at the forefront of any discussion about software reliability, because in the absence of perfect software,  reliability is a budgeting problem. We can only verify a subset of any system, so "why reliability?" turns out to be a red herring. The question to be thinking about is actually: "what do I most want to be reliable?"

[^fn1]: [Pragmatic Software Testing](https://www.oreilly.com/library/view/pragmatic-software-testing/9780470127902/), Rex Black

