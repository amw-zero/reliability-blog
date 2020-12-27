---
layout: post
title: 'State Explosion: the Reason We Can Never Test Software to Perfection'
tags: testing
author: Alex Weisberger
---

Have you ever seen a test suite actually prevent 100% of bugs? With all of the time that we spend testing software, how do bugs still get through? Testing seems ostensibly simple -- there are only so many branches in the code, only so many buttons in the UI, only so many edge cases to consider. So what is difficult about testing software?

# Branch Coverage is Almost Meaningless

> Consequently, drawing conclusions about software quality short of testing every possible input to the
program is fraught with danger.[^fn1]

When we think of edge cases, we intuitively think of branches in the code. Take the following trivial example:

~~~
if (currentUser) {
  return "User is authenticated";
} else {
  return "User is unauthenticated";
}
~~~
{: .language-typescript}

This single `if` statement has only two branches[^fn2]. If we wanted to test it, we surely need to exercise both and verify that the correct string is returned. I don't think anyone would have difficulty here, but what if the condition is more complicated?

~~~
function canAccess(user) {
  if (user.internal === false || user.featureEnabled === true) {
    return true;
  } else {
    return false;
  }
}
~~~
{: .language-typescript}

Here, we could have come up with the following test cases:

~~~
let user = {
  internal: false,
  featureEnabled: false,
};

canAccess(user); // ==> false

let user = {
  internal: false,
  featureEnabled: true,
};

canAccess(user); // ==> true
~~~
{: .language-typescript}

This would yield 100% branch coverage, but there's a subtle bug. The `internal` flag was supposed to give internal users access to some feature without needing the feature to be explicitly flagged (i.e. `featureEnabled: true`), but the conditional checks for `user.internal === false` instead. This would give access to the feature to all external users, whether or not they had the flag enabled. This is why bugs exist even with 100% branch coverage. While it is useful to know if you have missed a branch during testing, knowing that you've tested all branches still does not guarantee that that the code works for all possible inputs. This is very problematic, because input domains are combinatorial in nature and can be extremely large -- practically infinite in lots of cases.

This is why there are more comprehensive (and tedious) coverage strategies, such as condition coverage. With condition coverage you must test the case where each subcondition of a conditional evaluates to true and false. To do that here, we'd need to construct the following four `user` values (true and false for each side of the `||`):

~~~
let user = {
  internal: false,
  featureEnabled: false,
};

let user = {
  internal: false,
  featureEnabled: true,
};

let user = {
  internal: true,
  featureEnabled: false,
};

let user = {
  internal: true,
  featureEnabled: true,
};
~~~
{: .language-typescript}

If you're familiar with Boolean or propositional logic, these are simply the input combinations of a truth table for two boolean variables:

|internal|featureEnabled|
|-|-|
|F|F|
|F|T|
|T|F|
|T|T|

This is tractable for this silly example code because there are only 2 boolean parameters and we can exhaustively test all of their combinations with only 4 test cases. Obviously `bool`s aren't the only type of values in programs though, and other types exacerbate the problem because they consist of more possible values. Consider this example:

~~~
enum Role {
  Admin,
  Read,
  ReadWrite
}

function canAccess(role: Role) {
  if (role === Role.ReadWrite) {
    return true;
  } else {
    return false;
  }
}
~~~
{: .language-typescript}

Here, a role of `Admin` or `ReadWrite` should allow access to some operation, but the code only checks for a role of `ReadWrite`. 100% condition and branch coverage are achieved with 2 test cases (`Role.ReadWrite` and `Role.Read`), but the function returns the wrong value for `Role.Admin`. This is a very common bug with enum types -- even if exhaustive case matching is enforced, there's nothing that prevents us from an improper mapping in the logic.

The implications of this are very bad, because data combinations grow combinatorially. If we have a `User` type that looks like this,

~~~

type User = {
  role: Role,
  internal: Boolean,
  flagEnabled: Boolean
}
~~~
{: .language-typescript}

and we know that there are 3 possible `Role` values and 2 possible `Boolean` values, there are then 3 * 2 * 2 = 12 possible `User` values that we can construct. Even 12 isn't so bad, but these multiplications get out of hand very quickly for real-world data models.

This is the ultimate dilemma: testing with exhaustive input data is the only way of knowing that a piece of logic is entirely correct, yet the size of input data domains make that prohibitively expensive in most cases.

Also note that in these examples, neither static typing nor function purity prevented these bugs. While they are each useful in their own right and can prevent certain classes of bugs, neither of them can prevent logical errors outright.

So far, we've only considered pure functions. We showed that traversing all branches in a function does not prove that the function returns the correct value in response to all values within its domain. Functions are some of the simpler mathematical objects that exist, which is unfortunate for us. We'll take a look at how a stateful, interactive program gets much worse.

# State Expands the Domain of Functions and Adds Path Dependence to Interactions

A stateful, interactive program is more complicated than a pure function, and is most naturally modeled by a state machine. While input domains can be large, state graphs can be quadratically larger. Let's consider the [following stateful React app](/assets/user-form-app), which I've chosen because it has a bug that actually occurred to me in real life[^fn4]. 

~~~
type User = {
  name: string
}

const allUsers: User[] = [
  { name: "User 1" },
  { name: "User 2" }
];

const searchResults: User[] = [
  { name: "User 2"}
];

type UserFormProps = {
  users: User[],
  onSearch: (users: User[]) => void
}

function UserForm({ users, onSearch }: UserFormProps) {
  return <div>
    <button onClick={() => onSearch(searchResults)}>
      {"Search for Users"}
    </button>
    {users.map((asset => {
      return <p>{asset.name}</p>
    }))}
  </div>;
}

function App() {
  let [showingUserForm, setShowingUserForm] = useState(false);
  let [users, setUsers] = useState(allUsers);

  function toggleUserForm() {
    setShowingUserForm(!showingUserForm);
    setUsers(allUsers);
  }

  return (
    <div className="App">
       {<button onClick={() => setShowingUserForm(!showingUserForm)}>
          {"Toggle Form"}
        </button>}
      {showingUserForm && (
        <UserForm users={users} onSearch={setUsers}/>
      )}
    </div>
  );
}
~~~
{: .language-jsx}

This app can show and hide a form that allows selecting a set of Users. It starts out by showing all Users but also allows you to search for specific ones. There's a single list of results that is populated by whatever you have chosen to do. There's a top-level `App` component which maintains all of the state, and a `UserForm` component which simply renders data that it's passed.

There's a tiny (but illustrative) bug in this code. Take a minute to try and find it.

...

The bug is exposed with the following series of interactions:

1. Show the form
2. Search for a User
3. Close the form
4. Open the form again

At this point, the Users that were previously searched for are still displayed in the results list. This is what it looks like after step 4:

<img src="/assets/UserResultBug.png" style="margin: auto; max-width: 45%; border: 1px solid black" />

The app hardcodes the search results to always return User 2, and after opening up the form after searching that's what we see. The bug isn't tragic, and there's plenty of simple ways to fix it, but we just want to understand what went wrong. The best way to do that is to look at the state diagram of this application:

![](/assets/UserFormStateDiagram.png)
<img src="/assets/UserFormStateDiagramLegend.png" style="margin: auto; max-width: 60%;"/>

There are 2 state variables in this application: `showingForm`, and `users`. `showingForm` represents whether or not the form is showing, and `users` is the set of `User`s that the application is currently displaying. The edges between them represent the actions that a user can take. They're color coded, and  `ToggleForm` means they click the "Toggle Form" button, and `SearchForUsers` means they initiated a search for different `User`s. This state diagram was produced assuming there are only 2 `User`s in the system, namely `u1` and `u2`. Then, there is a unique state for unique combination of the state variables, i.e. there are two different states for `users = {u1}` and `users = {u2}`. 

This is probably an uncommon way to present the states of a program, but I think it's the most important. Often, especially with a user interface, we think about the _logical_ states a program can be in, e.g. we might speak of "Form Open" and "Form Closed" states for this application. Logic is absolutely specific, though, and can make decisions based on any discrete value whatsoever. By visualizing our state diagram this way, we can observe this problem:

![](/assets/UserFormStateDiagramTransitionHighlight.png)

Here we see that we can hide the form after the search returns `u2`, and then we can show the form and `u2` is still the only member of `users`. Note how if we never perform a search, we can never get into this state:

![](/assets/UserFormStateDiagramNoSearchTransitionHighlight.png)

The fact that the same user action (`ToggleForm`) can produce a correct or buggy result depending on the sequence of actions that took place before it means that its behavior is dependent on the path that the user takes through the state diagram. This is what's meant by _path dependence_, and it is a huge pain from a testing perspective. It means that just because you witnessed something work one time does not mean it will work the next time-- you potentially have to perform the exact same action at different points in time since the result might be different.


# Why is the state space complicated? - Path dependence

# Mimsmatch between Control Flow Graph and State Space

Programming languages are powerful because they allow us to describe large and complicated state spaces with relatively few lines of code. Ex: Deal Form bug in React vs. state space. I think this is exacerbated in client-server applications, where we're often just fetching and displaying data.

# Function Purity - Input space is a subset of the state space

Pure functions are in fact simpler, but that isn't the whole story. Purely functional programs can easily simulate state by passing accumulator parameters through recursive function calls. (Lamport Computation and state machines). 


In a typical web application, the database represents the current state of the application, and every endpoint call represents an action. This means that 

MC/DC - DO-178C

<hr>

[^fn1]: [A Practical Tutorial on Modified Condition / Decision Coverage](https://shemesh.larc.nasa.gov/fm/papers/Hayhurst-2001-tm210876-MCDC.pdf), Kelly J. Hayhurst, Dan S. Veerhusen, John J. Chilenski, Leanna K. Rierson.

[^fn2]: Of course this if statement could be written as a one liner using a ternary, but the branches are written to explicitly show the individual branches in the code.

[^fn3]: [Specifying Systems](https://lamport.azurewebsites.net/tla/book.html), Leslie Lamport

[^fn4]: There are ways of avoiding this bug, particularly by changing which component "owns" which part of the state. The actual application that I noticed this bug in was quite a bit larger than this example, and this design made more sense in that context. Inventing example code is hard, and this type of bug does occur often in practice, so I still think this example is worth taking a look at in this form.