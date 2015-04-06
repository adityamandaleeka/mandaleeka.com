---
layout: post
title:  "Know your tools"
date:   2015-03-31 18:59:20
categories: tools
---

Tools are awesome. They make the world go round. There's nothing like having the perfect tool for a job, and conversely, few things are worse than having to solve a problem when you have no suitable tools (and believe me, when you work on developer tools, you often [have no tools because you've destroyed all your tools with your tools](http://research.microsoft.com/en-us/people/mickens/thenightwatch.pdf)).

That being said, it's important to understand what your tools are doing and how they work. This seems obvious, but it's really easy to fall into the trap of having a flawed mental model of how a tool works and then being misled when that model turns out to be false. 

Here's a quick example of how even a debugger can mislead you if you're not careful.

Say you have some C++ with a vector and a reverse iterator over it. Nothing complicated.

{% highlight C++ %}
int currInt = 0;
std::vector<int> myVector { 1, 2, 3, 4, 5 };
for (auto reverseIter = myVector.rbegin();
     reverseIter != myVector.rend();
     ++reverseIter)
{
    currInt = *reverseIter;
    // do important stuff here
}
{% endhighlight %}

During execution, you set a breakpoint on the line where `currInt` is being set and hover over `reverseIter`. The debugger shows you a helpful tooltip that says:

{% highlight C++ %}
|reverseIter | reverse_iterator    current 3|
{% endhighlight %}

So what will the value of `currInt` be after executing this line of code? 

It's really easy to say 3 because, well, of course it is. The debugger is clearly telling you that, and the debugger is your friend, right?

Well actually, the answer is really 2, and before you say it, this isn't a bug in the debugger. What it showed was correct, but it was easy to miss a crucial word in that tooltip.

Looking at the official C++ standard, we find that the word `current`, which the debugger showed us and we ignored, means something very specific in this case. `reverse_iterator` has a member called `current` which points to the element one position _after_ the element the iterator points to. The reason why `currInt` gets set to something different from `current` is because `operator*` does the following:

{% highlight C++ %}
Effects:
       Iterator tmp = current;
       return *--tmp;
{% endhighlight %}

One day, we'll have tools that can read your mind, understand your problem, and show you exactly what you need to see. Until then, no matter how tempting it is to believe we can blindly rely on our tools, it's scarily easy to misjudge what they are saying[^profiler].

Know your tools and stay awake.

[^profiler]: This problem is even worse when the tool in question is something like a profiler, in which case there are so many potential things to misunderstand. You need to have a basic handle on how it collects data, how it displays the data it collects, whether the way your program was behaving when you profiled it was representative of it's usual state, whether the very presence of the profiler has a performance impact on your program, and so on.