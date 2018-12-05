Here are some applications of #reduce:

The most basic use of #reduce is its go-to use of adding an array of numbers
This can be done with a block

```ruby
(5..10).reduce { |sum, n| sum + n }
=> 45
```

Or without a block

```ruby
(5..10).reduce(0, :+)
=> 45
```

It seems superfluous pass a value to the argument when you are adding, but essential if you are using it to multiply.

```ruby
(1..10).reduce(1, :*)
=> 151200
```ruby

This can also be achieved using a block:

```ruby
(1..10).reduce(1) {|product, n| product * n}
=> 151200
```

Creatively, #reduce can also be used to perform an iterative action if it involves comparison, or accumulation.

```ruby
%w(cat mouse donkey).reduce do |memo, word| 
    memo.length > word.length ? memo : word
end
=> "donkey"
```

```ruby
"How are you?".chars.reduce { |memo, char| %w[a e i o u y].include?(char) ? memo + char * 5 : memo + char }
```

We can also use #reduce to turn an array into a hash.

Remember this is one of the ways Ruby processes #reduce: 

```ruby
reduce(initial) { |memo, obj| block } → obj
```

```ruby
%w(a b c).reduce({}) {|memo, obj| memo.update(obj => obj.ord)}
=> {"a"=>97, "b"=>98, "c"=>99}
```

Note that #update is the same as #merge, and is used to update a hash.

update(other_hash) → hsh
update(other_hash){|key, oldval, newval| block} → hsh

You can achieve the same effects with 1) map/each, and 2) each_with_object

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
This is an example of how reduce can be used in a procedural Fibo problem.

```ruby
p (3..10).reduce([1, 1]){|(a, b), _| [b, a+b]}
# => [34, 55]
p (3..10).reduce([1, 1]){|(a, b), _| [b, a+b]}.last
# => 55
```

This is how #reduce is introduced in Ruby docs:

Ex: Sum some numbers `(5..10).reduce(:+) #=> 45`

Ex: Same using a block and inject -->`(5..10).inject { |sum, n| sum + n } #=> 45`

Ex: Multiply some numbers -->`(5..10).reduce(1, :*) #=> 151200`

Ex: Same using a block -->`(5..10).inject(1) { |product, n| product * n } #=> 151200`

Ex: Find the longest word

```ruby
longest = %w{ cat sheep bear }.inject do |memo, word|
   memo.length > word.length ? memo : word
end
longest                                        #=> "sheep"
```
