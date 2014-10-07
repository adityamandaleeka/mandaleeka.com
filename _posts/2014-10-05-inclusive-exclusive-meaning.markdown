---
layout: post
title:  "'Inclusive' and 'Exclusive'"
date:   2014-10-05 18:59:20
categories: profilers
---

While observing people using performance tools, I've seen some confusion around the terms 'exclusive' and 'inclusive' (some tools call them 'self' and 'total'). The distinction between the two is simple, but understanding it is important in order to use your profiling data effectively. 

You're likely to see these terms in tools like CPU profilers which show you what was on the stack over a period of time. Here's what the difference is: costs (generally in units of samples for sampling-based tools or units of time for instrumentation-based tools) associated with code anywhere on the stack fall under the 'inclusive' category while costs associated with code at the top of the stack go under the 'inclusive' category _and also_ the 'exclusive' category.

Consider the following simple call tree from a sampling profiler. Remember, a sampling profiler walks and collects the call stack at some interval during program execution and aggregates information from all the stacks it collects in order to present a statistical approximation of where time was spent during program execution.

{% highlight C++ %}
Function       Inc Samples    Exc Samples
foo()                  100              1
|__bar()                50             20
   |__baz()             18             18
   |__quux()            12             12
|__baz()                49             49
{% endhighlight %}

The inclusive count for `foo` in the tree represents the number of samples spent in `foo` and its children. In this case, because `foo` and its children account for the entirety of the tree, we see that there were 100 samples taken in total. Of those, only 1 was in `foo` itself and the rest of the samples were taken when one of the functions it called (or one of the functions _they_ called) was executing.

Moving down, we see that `bar` is the next highest in terms of inclusive samples with 50, of which 20 were exclusive samples while the rest were in its children. We can also see that `baz` took up 49 of our 100 total samples and since it has no callees, all that time was spent inside `baz` itself. Thus, while `foo` has the largest count of inclusive samples, and `bar` has about the same as `baz`, making improvements in `foo` or `bar` will be of limited use compared to making `baz` faster. I'm not talking about improving the way other functions are called from `foo` or `bar` here, of course; if `foo` is calling `baz` in a loop millions of times unnecessarily, then fixing that will also reduce the amount of time spent in `baz`. As always, it's important to have a reasonable understanding of the flow of the code you're profiling in order to solve performance problems using tools.

While this was a very simple example, hopefully the next time you use a profiler you will have a better understanding of which functions will give you the best "bang for the buck" when optimizing.