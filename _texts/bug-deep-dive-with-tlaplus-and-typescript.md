
The assertions in this test fail to catch an error: https://github.com/amw-zero/deal-mangement/blob/master/src/dealManagement.test.js#L141-L143

This application is a table which displays "Deals," and a form which creates Deals to populate the table. Deals can be associated with Assets, and a bug is introduced where searching for an Asset sets some mutable state and it never gets cleared out, so when you re-open the form it shows the previous search results instead of the full set of assets.

I actually unintentionally wrote this bug.

This is a prime example of an "Inappropriate or missing action," as described in the paper "Towards a Theory of Test Data Selection": http://barbie.uta.edu/~mehra/38_toward%20a%20theory%20of%20test%20data%20selection.pdf

Tla+ Modeling, show finding the error with model check

An interesting observation arises from Looking at this in TLA+, namely what mutability means in the context of state transitions. Mutation makes all of the non-mutated parameters implicity UNCHANGED in TLA+ speak. Now, whether or not immutability actually solves any problems is up for debate, but this may be a pro for immutability: it does force you to think about what the value of each parameter should be at each point of change. However, many functional languages provide an immutable update syntax where you only override one parameter at a time. In this specific case, I see no benefit to immutability. The problem lies at the heart of the algorithm, not with any operational concern. Conversely, by forcing us to explicitly say what the value of each variable is at each state transition, TLA+ does force us to consider it, even though it gives us a trivial escape hatch in UNCHANGED. 

By sweeping the state space and enforcing constraints globally, we can hone in on exactly where we missed an action - we forgot to properly set the selectedAssets back to an empty array before re-showing the form.

Typescript: Show how to possible avoid this error by restricting the number of states the UI can be in, compare broken and fixed state graphs produced by TLC