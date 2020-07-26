---
layout: narrative
title: 'Input space: the final frontier'
tags: test
author: Alex Weisberger
---

> Consequently, drawing conclusions about software quality short of testing every possible input to the
program is fraught with danger.[^fn1]

An extremely important concept in my thinking about quality is the role of input data in program correctness. The concept can be stated in the following axiomatic form:

**The Axiom of Input Totality:** _A program is correct if and only if it behaves correctly in response to all possible inputs._

It's worth precisely defining "behavior" as well:

**state:** _an assignment of values to variables_

**action:** _an externally observable event_

**behavior:** _a sequence of states and actions_[^fn2]

Fundamentally, this is what we are doing when we write programs: creating sequences of states and actions.

We may not think about all programs as accepting explicit inputs. For example, what are the inputs in a long-running, stateful, interactive program, such as the average enterprise application? Such a program can be thought of as building up its inputs piecewise over time by storing the result of interactions as state, e.g. entering data via forms or clicking buttons which trigger business logic. The current state of the system then becomes an implicit input to many of its operations. (make consistent with following section about Beyond Pure functions)

This is bad news for reasoning about program correctness, because state spaces can be astronomically large. It's fairly straightforward to count the number of elements in state space sets, and consequently input space sets, so we'll see that this isn't hyperbolic. The frightening implication of this is that we often test using an infinitesimal subset of the input space, and as the opening quote warns, we should be very wary of the conclusions we make from such a small subset. Observing the correct behavior for a few inputs does not imply it will continue to be correct for all inputs. This is why the concept of input spaces has been on my mind so much recently, because it illuminates both the essence of functional correctness and its biggest challenge-- we expect a program to behave as expected throughout all of the possible interactions we have with it.

The simplicity of this ask is horribly deceptive, and unfortunately all signs point to it being completely infeasible to ensure in the general case.[^fn3] We simply have to try and find intelligent input space subsets that are representative of the whole.

## The intuition of input spaces

When we talk about an input space, we're talking about the domain of a function, which is the set of all values that the function can take as input. Let's look at a simplified representation of the `ls` program, which takes in a path to a directory and returns a list of files in that directory:

~~~
function ls(path: string): string[] { 
  ...
};
~~~
{: .language-typescript}


What is the input space of this function?

Here, `ls` takes in a single string. We immediately hit a wall: there are infinitely many strings. Of course, computers are physical machines, so we do have limits on the number of characters that can be in a string within a computer program. Let's represent the upper bound of the length of a string with _S_, and for simplicity let's assume that only lowercase English letters can be used. With these constraints, we can represent 26^_S_ individual directory paths which means it only takes _S_=7 to get us into the billions-of-possible-strings territory. Real strings have much higher length upper bounds and allow many more characters, making the real-world possibilities even larger. 

By the Axiom of Input Totality, we're on the hook for making sure every single one yields the right result.

## Data access

Data access control is one area where correctness isn't a nice-to-have. If data is sensitive enough to have conditional access rules, then it's a big problem if someone sees it when they shouldn't. Data leaks aren't met with understanding from customers. They're met with lost business and lawsuits. Let's look at a simple representation of attribute-based access control[^fn4] for a resource and analyze its input space:

~~~
interface User {
  id: number
}

enum AccessLevel {
  Read,
  ReadWrite
}

interface UserAuthorization {
  user: User,
  accessLevel: AccessLevel
}

interface Resource {
  userAuthorizations: UserAuthorization[]
}

function canUserAccess(user: User, resource: Resource) {
  ...
}

let user = { id: 1 };
let userAuthorization = { user, accessLevel: AccessLevel.Read };
let resource = { userAuthorizations: [userAuthorization] };

canUserAccess(user, resource);
~~~
{: .language-typescript}

What is the size of the input space of `canUserAccess`? Though this function is immediately more tricky with it taking in composite data types, we can arrive at its input space with a few simple rules. Operating on types like these is a much more realistic scenario in an enterprise information system, so it's very useful to know how to perform this kind of analysis. The end goal is to determine how many `User`s and how many `Resource`s can possibly be represented, since `canUserAccess` takes in one value of each type. We want to know this because, you guessed it, this function can only be correct if it handles all possible input values correctly.

Let's introduce a function, _n(T)_, which yields the number of possible values that a given type _T_ can represent.[^fn5] For example, _n(AccessLevel)_ = 2, because there are only 2 possible `AccessLevel` values: `Read` and `ReadWrite`.

Let's also assume that there are _U_ `Users` in our database at a given time (this will help express _n(UserAuthorization)_ and _n(Resource)_, as we'll see in a second).

The fields of record types can vary independently, meaning we can use the rule of product to determine their total possible values, i.e. _n(UserAuthorization)_ = _n(AccessLevel)_ * _n(User)_ = 2_U_.

Now, `Resource` has a single field, an array of `UserAuthorizations`. Determining the number of possible arrays is a confusing problem at first glance. Arrays are dynamic and flexible data structures, and you can create Arrays of arbitrary sizes. We might be tempted to think that _n(Array)_ is infinite. However, again there is a practical bound on a flexible data type, and here we are bound by _U_-- there's no need to consider the access levels of users outside of the system.

The problem is then reduced to how many different arrays of length _U_ there can be. We can consider an array to be equivalent to a mathematical set in this case, and we know that the set of all possible subsets of a set is also known as its power set. The power set of a set _S_ with _n(S)_ = _N_ has 2^_N_ elements. Finally, _n(Resource)_ = _n(UserAuthorization[])_ is then 2^_n(UserAuthorization)_ = 2^2_U_.[^fn6]

To get a sense of how the number of possible `Resources` grows with the number of `Users` in the system, observe the following table:

|U|n(Resource) = 2^2U|
|5|1.02e+03|
|10|1.05e+06|
|15|1.07e+09|
|20|1.10e+12|

At 20 `Users`, there are over 1 trillion possible `Resource` values.

# Beyond pure functions

The final reason that I think understanding input spaces is so crucial is that input space and state space are inextricably linked. What exactly is meant by state space?The input space and state space are intricately linked. While the input space determines what can be passed into individual functions, it also determines the individual states that the program can be in over the course of time. Let me explain.

Back to our definition of "behavior," which is a sequence of states over time. 

The data access story doesn't exist in a vacuum. Data access is actually a set of potential behaviors. In the context of a client-server application, these operations would mostly likely take place on the server behind an endpoint. For example:

AddUserToResource
RemoveUserFromResource
ModifyAccessLevelOnResource
CanUserAccessResource

The input space is the set of possible states that can be transitioned to, but the number of actual transitions depends on the requirements of the behaviors.

Now, we don't need to test one trillion users to detect bugs. Daniel Jackson coined the phrase "small scope hypothesis," (citation, Software Abstractions) which means that we can test these operations for a small _U_. We probably want to introduce _R_ as the number of Resources in the system as well, so that we can test a few users getting added and removed from a few different resources.


# Closing Thoughts

Software is truly powerful and flexible, as the simplified data access example highlights. The values that our logic operates on can  but that flexibiliy comes at a cost: reasoning about this many possibilities is exhausting, and reasoning is a requirement for correctness.

While many problems related to input spaces seem like they're infinite or too large to comprehend, placing bounds on certain dynamic data structures makes these problems more tractable. This dovetails into the small scope hypothesis, which says that we can use these bounds to still find lots of bugs.

---

Category partition method.

Small scope hypothesis - limiting a problem to an upper bound. Show alloy data model.

DO-178C (avionics certification) + modifed condition / decision coverage


[^fn1]: [A Practical Tutorial on Modified Condition / Decision Coverage](https://shemesh.larc.nasa.gov/fm/papers/Hayhurst-2001-tm210876-MCDC.pdf), Kelly J. Hayhurst, Dan S. Veerhusen, John J. Chilenski, Leanna K. Rierson.

[^fn2]: 
    In [Specifying Systems (pg 15)](https://lamport.azurewebsites.net/tla/book.html), Leslie Lamport comes up with what I think is the best possible description of software behavior:

    > Formally, we define a behavior to be a sequence of states, where a state is an assignment of values to variables.

    I've augmented this definition to include "actions," which is a less frightening term for side effects, i.e. sending emails, writing to a database server, triggering SMS or mobile push notifications, etc. Actions are essential in real-world systems, so we can't leave them out in the name of mathematical purity.

[^fn3]: [Why Writing Correct Software is Hard... and why math (alone) won't help us](https://pron.github.io/posts/correctness-and-complexity#chapter-ii-complexity). This is an amazing walk through computing history, explaining how the limits of computation were found early on, and how counterintuitive they are.

[^fn4]: [Attribute-based access control](https://en.wikipedia.org/wiki/Attribute-based_access_control) is probably the most useful data access paradigm for businesses, as it allows you to encode rules using attributes of the user and resource to determine the access decision.

[^fn5]: Types are effectively sets of possible values, and _n(T)_ yields the cardinality of the set.

[^fn6]: I explained the reasoning behind each analysis, but these become easier over time. You use the rule of product to compute _n_ for a record type. _n_ of an enum type is simply equal to the number of variants there are. And _n_ of an array or list is equal to 2^_B_ where _B_ is some practical bound on the data that can be stored in the array.
