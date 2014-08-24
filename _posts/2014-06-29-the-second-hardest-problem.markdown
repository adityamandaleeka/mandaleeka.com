---
layout: post
title:  "The Second Hardest Problem"
date:   2014-06-29 18:59:20
categories: naming
---

There's an old saying that the two hardest problems in computer science are cache invalidation and naming things. 

It's not just a funny aphorism--years of working on large codebases has taught me that unclear or imprecise naming is a giant red flag, as if your surgeon were to say he needs to move some stringy bits over and pull out the squishy blob. Be worried.

The most obvious reason to care about naming is that, with very few exceptions, your code has to be maintained. Whoever comes along to fix or modify something (even if it's just future-you) is going to have a much easier time understanding what's going on if everything is named sensibly.

But that's an investment for the future, right? Nobody argues that poor names are better than precise ones. Good naming is right up there on the clean code checklist with stuff like having unit tests and not copying and pasting code. You have a feature to ship today, though, so you can't be bothered with future-looking best practices nonsense. Well, the most important reason to care about naming has nothing to do with the future, and it's why I encourage everyone to make the effort to get it right. 

Naming reflects thinking. If you can't come up with a descriptive and precise name for a function, chances are that you don't understand what it does. If you can't figure out what to call a variable, you're probably using it wrong too. Few things are worse than reading through an unfamiliar codebase and finding a function which appears, on the surface, to be doing what its name suggests, only to find out on closer inspection that the person writing it misunderstood what it was really doing and the whole system was working purely because of an unrelated coincidence. Raise your hand if you've seen that before.

The other benefits of focusing on naming are endless. Good naming can obviate the need for many types of comments, it can help to quickly identify edge cases and potential logical flaws, and can help people reviewing your code follow along.

These days I spend time and energy to get naming right, even to the point of discussing challenging naming problems with other people. The conversations may even lead to improvements to design decisions. It's time well spent.