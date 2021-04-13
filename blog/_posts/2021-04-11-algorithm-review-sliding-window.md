---
layout: post
title: Algorithm review - Sliding Window
category: blog
---

What is the general idea behind this technique?

With sliding window, we are typically working with a problem description that requires some sort of string manipulation that returns a contiguous substring, completed over a time complexity of O(n). 

In other words, we want a result that returns, in some shape or form (could be a string, array, or even boolean value) continuous substring(s) from the original string itself. 

With brute force, we would typically double loop the operation so as to scan each element of the string against the next element in the string, resulting in a time complexity of O(n^2).

Sliding window is a technique that sets up a shrinking window that examines the state of elements within the range. By having two pointers, each representing the beginning and end of a window, we can move the beginning pointer up or end window down, depending on what is asked.

How would we set up the pointers for a sliding window?

Typically, the front pointer would point to the beginning of the string, moving from left to right depending on how it would satisfy the condition. The end pointer also starts at the beginning, and moves to the right as long as the target condition is satisfied. 

If the target condition is broken (e.g. if the sum/product is greater/less than target, hits a number different than target, number of counts greater than target), then we move the left pointer up one to see whether the condition is reached. Note that we typically use the `while` condition instead of `if`, because we would not know ahead of time how many times the left pointer would need to move up in order to satisfy the condition.

If the question asks for a min/max/greatest/least value, then we also need to set up a variable that tracks this checked value across the iterations, replacing the return value at every turn when condition is met.

What are some data structures that can be added during this type of problem?

The `hash` data structure is often needed when solving a problem that requires `n` time complexity and can be solved with sliding window. 

For example, if we are tracking the total number of occurences of letters in a target string,   then before we can iterate over the problem string, we need to record the target pattern in a hash. Adding a hash structure requires us to keep track of the counts in the hash. So for every rightward movement of the start pointer, we'll also need to adjust the counts in the hash structure.

What are some potentially confusing aspects during the implementation process?

The right pointer in this case is best represented by a `for` instead of a `while` loop. The reason being that the right hand pointer will always want to move towards the end, while the left pointer moves or stops depending on the condition. Having two `while` loops might cause unintended mistakes, if we forget to move the right pointer manually at the end of every iteration.

In order to keep track of whether a target condition is reached, we typically enjoy an integer or a hash. Make sure that with every movement of the front window (and of course the back pointer), we are adjusting the value properly in those variables.

In some cases, we delete a hash key-value pair when the value reaches zero. In other cases, it might make sense to keep the pair even when it does reach zero.

We also need to make sure to understand the nuances of the question itself. Key words like "at least", "at most" indicates whether the condition checking requires an equal or a less than or greater than sign. Those are places where mistakes can be made.