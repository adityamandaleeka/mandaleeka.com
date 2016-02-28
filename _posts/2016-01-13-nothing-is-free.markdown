---
layout: post
title:  "Nothing is free"
date:   2016-01-11 18:59:20
categories: performance, development
---

As programmers, we are constantly standing on the shoulders of others. Whether you're writing client-side web stuff or a kernel-mode driver, you have to build a reasonable mental model of the layer below you and proceed based on that model. If that mental model is too vague, you won't get anywhere, and if you try to understand every minor detail of every layer that's below you, you will never get started on solving your actual problem. Striking a balance here is key, but sometimes it's hard to know what's important and what's not.

As it turns out, when tuning for performance, a lot of 'unimportant' things really do matter. Sure, your language/platform/framework might hide some complexity in an effort to make it easier to get things done, but ultimately, there is one overarching rule when it comes to performance: nothing is free.

Consider a very simple example. You have some data at memory location `x`. There is only one memory location `x` in your process's address space. You know that. That's how the system works, and it wouldn't make sense otherwise.

Well, several levels down the stack, you have the hardware that this thing is running on. Here, there may be multiple caches all containing either what _is_ the value at `x` or _was_ the value at `x` at some recent point in the past. There's some magic gnome running around at this level managing these copies for you so that every thread in your program (if it's written properly) is reading/writing the correct value at each point in time. This is called cache coherency, and it's happening automatically. You never really have to worry about it being inconsistent, but it's not free. Nothing is free.

For a nightmare scenario demonstrating how _not free_ it can be, consider the situation in which you have two threads running on different cores that are not even accessing the same data. Thread 1 is using data at memory location `m` and Thread 2 is using data at another location `m+n`. Those are two distinct locations, and from each thread's perspective, nobody else is going to be writing to the location that it cares about. Surely, nothing can go wrong at this point.

Well, _as it turns out_&trade;,  if `n` is not big enough, you're going to have a bad time because `m` and `m+n`, which may be holding completely unrelated values from a program logic standpoint, will end up on the same cache line. As each thread writes to its own variable, that write invalidates the cache line in the other core that happens to contain _its_ variable, and vice versa, and you're now spending precious time updating cache lines to reflect changes in values you don't care about. You didn't think you needed to worry about this, but nothing is free.

Now, I'm not suggesting that you start stressing out over all the things that could potentially be going wrong in every layer of your program down to the metal. If you're lucky, you may never even hit a performance problem that's caused by a really low-level detail like this. For most classes of software, it's probably not going to be noticeable even if it does occur; there are usually other bottlenecks that are more significant. But apart from just being aware of this class of problem, it's also useful to remind yourself the next time you want to add yet another framework or library to your stack that you probably don't need and definitely don't fully understand: nothing is free.