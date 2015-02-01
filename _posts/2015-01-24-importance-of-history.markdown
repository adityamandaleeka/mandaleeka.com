---
layout: post
title:  "History"
date:   2015-01-24 18:59:20
categories: development
---

History is underrated in our community.

This is true on a macro level, in that most developers have never studied the history of the field as a whole, but it's also true on a micro level, in every software project that is active for any nontrivial amount of time.

If you talk to developers working on different projects and in different companies you'll notice a reverse [Lake Wobegon effect](http://en.wikipedia.org/wiki/Illusory_superiority) going on; everyone believes that their codebase is a mess and that parts of it need to be thrown away and rewritten. While this is sometimes because the code in those parts is smelly or poorly architected, it is often because the approach that was taken to solve the high-level problems seems fundamentally wrong. 

At the same time, any developer worth their salt knows that it's generally not a good idea to throw away large components and rewrite them from scratch without (a) a good reason to and, crucially, (b) a solid understanding of the old code that's being thrown out. It's easy to think that a chunk of code in some routine is doing something that's completely pointless, but the fact that someone put it there in the first place means that at least one person thought it served a purpose (more than one person if you have a [code review process](/development/2014/11/01/code-reviews-and-you/), which you should). There are many possible explanations for its presence:

* Perhaps it has a legitimate purpose and you're not seeing the big picture.
* Maybe it made sense at some point but something somewhere else changed since then, making it redundant.
* The person who wrote it was in fact a moron[^moron].

There are other possibilities too, but the point is that you don't know which is the case unless you have solid context for the existence of that code. Unfortunately, that generally means you had to be the one who wrote it. If that person is no longer around, then tough luck; you can either avoid the risk of inadvertently breaking things you don't have tests for by keeping the moldy old code around, or you can bite the bullet, perform a costly rewrite, and take the chance that someone will send you hatemail because you broke support for their [favorite scenario on their pet platform](http://xkcd.com/1172/), at which point you'll have to debug some hard-to-reproduce race condition which leads you, after much pain, to the reason why that chunk of code was there in the first place.

Clearly, neither of those choices (being paralyzed by fear, or being reckless) is ideal, and we can't start forcing anyone who writes any code to answer phone calls about it long after they've moved on. Comments in code are great, but they only get you so far; they don't tell you what alternative approaches were considered, what were the explicit goals and non-goals for that component at the time it was written, whether there were some impending deadlines that may have affected decision-making, and other stuff that you can only find out from someone with a good memory who was around at the time.

I don't know what the perfect solution to this problem is. I know that some teams take good notes on everything that's being done and keep a log of all the options and proposals that are considered, along with reasons why they were or were not implemented. The idea is that if someone comes along and has an idea, chances are that someone had that idea earlier, and by searching this log they can find out why it was a bad idea in the past (or not). This approach makes more sense for some projects than for others, and it can be heavyweight if not done right, so it's not a perfect solution.

We need to get better at this and figure out best practices on our own projects and teams that will ensure we're not entirely dependent on people and their memories in order to leverage the learnings of the past.

[^moron]: This is generally unlikely, but if you find that it's a frequent occurrence, then maybe you should find some new people to work with.