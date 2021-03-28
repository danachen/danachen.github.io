---
layout: post
title: A deeper look at Ruby's Enumerable#reduce
category: blog
---

When first learning about collections and the Enumerable module in Ruby, like most beginners, I focused on understanding and applying #each, #map, and #select. It's not until months down the road, while browsing problem solutions by Rubyists, that I began to discover the power of the #reduce method and began digging deeper. 

In this post, I want to articulate my understanding of the method and its applications at this moment. I hope to continue updating my mental model of both the method itself, and the paradigm that it represents (Map/Reduce in functional programing). 

## Why #reduce?

I began this exploration by first trying to understand the problem #reduce is built to solve.  

Enumerable#reduce, like the rest of the Enumerable methods, work with collections. 

But unlike #each or #map, where a collection is returned at the end, #reduce performs a different kind of function over a collection. True to name, it "reduces", or accumulates elements of the collection to a single value based on a binary operation, and returns one value at the end. 

## #reduce syntax

At first glance, #reduce seems to work with the same type of syntax as other members of the Enumerable class. 

Let's compare #reduce with #map for the most basic usage. 

This is how a #map method works its way across a collection: the collection takes an enumerable object, in this case an array, and executes the block by passing in each element as an argument. The result of evaluating the block is returned as an array. 

```ruby
(5..10).map { |num| num * 2 }
#=> [10, 12, 14, 16, 18, 20]
```

The most basic use of #reduce is its go-to use of "reducing" an array of elements with a block.

```ruby
(5..10).reduce { |sum, n| sum + n }
#=> 45
```
This looks close enough to a #map function, the only difference being that there are two elements passed in as argument for each step of the collection. We'll look at those in detail in a sec, but in the meantime. we've also seen #reduce written in shorthand like this, without a block.

```ruby
(1..5).reduce(1, &:*)
#=> 120
```
Or like this, without the 0 (accumulator).

```ruby
(1..5).reduce(&:*)
#=> 120
```

Or like this, without even the ampersand. 

```ruby
(1..5).reduce(:*)
#=> 120
```

So what is essential, and what is superfluous, when it comes to using the #reduce method?

The Ruby documentation mentions three different inputs, which I'm going to refer to as accumulator(initial), operator(sym), and block. When passed in as arguments, memo refers to the accumulator, and obj is the current value iterated over. 

```ruby
reduce(initial, sym) → obj
reduce(sym) → obj
reduce(initial) { |memo, obj| block } → obj
reduce { |memo, obj| block } → obj
```

At the beginning of a method, we have three values that can be passed in: an accmumulator that is the starting value of the series of binary operation performed over a collection, an operator that defines the operation to be performed, and a block that can further define what needs to be performed.

We can play around with the method by reading the error messages it throws.

1. Provide no block and no operator.

```ruby
(1..5).reduce
#=> no block given
```
The error message tells us that a block is expected in this case.

2. Provide only the accumulator, no operator or blocks.

```ruby
(1..5).reduce(1)
#=> 1 is not a symbol nor a string
```
Error message suggests in the absence of a block, it's expecting either a symbol or a string to act as the operator.

3. Pass in a method #to_f to where a symbol is expected.

```ruby
(1..5).reduce(:to_f)
#=> wrong number of arguments (given 1, expected 0)
```
This confirms that the method only accepts a certain type of method, and #to_f isn't one of them.

4. What if we try to pass in an operator and a block at the same time?

```ruby
 (1..5).reduce(&:*) {|memo, obj| memo += obj}
#=> both block arg and actual block given
```
It looks like we can't pass in both an operator and a block. This could be because the & turns the :+ method into a block, which then conflicts with the block that's also getting passed in. 

But what if we change the code slightly to the following:

```ruby
 (1..5).reduce(1, :*) {|memo, obj| memo += obj}
#=> 120
```
By specifying the accumulator value, and leaving the method as a symbol, we can actually pass in both an operator and a block at the same time. 

Which command will the program run? The operator or the block?

```ruby
 (1..5).reduce(1, :*) {|memo, obj| memo += obj}
#=> 120
```
It looks like the operator takes precedence over the block.

After all that spilled ink, it looks like #reduce method can show up in one of four flavours:

1. Takes a block and an accumulator
`(1..5).reduce(1) { |memo, obj| memo *= obj }`
2. Takes no parameters, only block
`(1..5).reduce { |memo, obj| memo *= obj }`
3. Takes an operator and an accumulator
`(1..5).reduce(1, :*), or (1..5).reduce(1, &:*)`
4. Takes an operator only (in symbol or block form)
`(1..5).reduce(:*), or (1..5).reduce(&:*)`
*5. It accepts both an operator and a block, if the operator is passed in as a symbol, and processes the operator. But why?

Moving on, let's see how it can be used!

## Using #reduce to solve problems

TL;DR: #reduce is particulalry effective when it comes to accumulating values across an array (accumulating not just in a numerical sense, but also to compare and select other input types such as a string), and is also handy when we want to dynamically build a new object from a collection based on another set of conditions. 

1. Let's see how #reduce can also be used to perform an iterative action if it involves comparison, or accumulation.

Problem: Given an array of words, find the longest word.

Solution using #reduce:  
```ruby
%w(cat mouse donkey).reduce do |memo, word| 
    memo.length > word.length ? memo : word
end
#=>"donkey"
```
Combined with a ternary operator, #reduce stores the sought after value in its memo parameter, returning the value at the end. 

Problem: Given a string, make it shout by duplicating every vowel 5 times. 

Solution using #reduce:
```ruby
"How are you?".chars.reduce { |memo, char| %w[a e i o u y].include?(char) ? memo + char * 5 : memo + char }
#=> "Hooooow aaaaareeeee yyyyyooooouuuuu?"
```
Chained after a #char method, #reduce uses a ternary operator again to perform what's passed in through the block.

2. We can also use #reduce to turn an array into a hash.

Problem: Given an array of letters like [a, b, c], return an array that shows each array element pointing to its ASCII equivalent. (Hint: make use of #ord and #update)

Solution using #reduce: 
Remember this is one of the ways Ruby processes #reduce:
`reduce(initial) { |memo, obj| block } → obj`

```ruby
%w(a b c).reduce({}) {|memo, obj| memo.update(obj => obj.ord)}
#=> {"a"=>97, "b"=>98, "c"=>99}
```

A hash is initialized as the accumulator, hash entries are added while working through the collection. Note that update is the same as #merge, and is used to update a hash.

```ruby
update(other_hash) → hsh
update(other_hash){|key, oldval, newval| block} → hsh
```

We can achieve the same results with 1) map/each, and 2) each_with_object, is #reduce better or worse?

```ruby
hsh = {}
  %w(a b c).map do |item, num|
    hsh[item] = item.ord
  end
  hsh
```

```ruby
hsh = {}
  %w(a b c).each do |item, num|
    hsh[item] = item.ord
  end
  ruby
end
```

```ruby
%w(a b c).each_with_object(hsh = {}) do |(k, _), _|
  hsh[k] = k.ord
end
hsh
```
3. Building an array as you go.

Problem: Given an array of integers, return an array of even numbers in string form.

Solution using #reduce:

```ruby
[*1..6].reduce([]) do |new_arr, el|
  new_arr << el.to_s if el % 2 == 0
  new_arr
end
#=> ["2", "4", "6"]
```
Similar to the building-a-hash as you go example above, this time we are building an array conditional on another set of properties. 

4. #reduce can also be deployed in a procedural Fibo problem. This is not the most intuitive use of #reduce, this is but a demonstration that it is possible.

Problem: Find the 10th Fibonacci number. 

Solution: this can be solved in a variety of ways, #reduce is but one of them.

```ruby
p (0..8).reduce([0, 1]){|(a, b), _| [b, a+b]}.last
p (3..10).reduce([1, 1]){|(a, b), _| [b, a+b]}.last
# => 55
```

Using good old iteration. 

```ruby
def fibo(n)
  a, b = 1, 1
  counter = 0
  while counter <= n
    a, b = b, a+b
    counter += 1
  end
  a
end

p fibo(10)
# => 55
```

The same results with #recursion.

```ruby
def fibo(n)
    return n if (0..1).include?(n)
    fibo(n - 1) + fibo(n - 2)
end

p fibo(10)
# => 55
```

Using #each_with_object also does the job.

```ruby
def fibo(n)
  (n - 1).times.each_with_object([0, 1]) { |num, obj| p obj << obj[-2] + obj[-1]}.last
end
```
