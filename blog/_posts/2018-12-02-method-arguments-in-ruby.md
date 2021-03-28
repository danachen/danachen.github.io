---
layout: post
title: Method arguments in Ruby
category: blog
---

Ruby allows for great flexibility in how arguments can be passed into a method.

There are 3 categories of arguments that get passed into a method, depending on how you look at them:

1. Required arguments
2. Arguments with default values
3. Optional arguments

Ex1: required arguments are pretty straightforward

``` ruby
def any_old_method(param)
   param
end

any_old_method(‘Hi there!’)     #=> ‘Hi there!
```

Ex2: args with default value, note the default value is optional

``` ruby
def method_with_default_value (a, b = 2018)
  "It's #{a} in #{b}."
end

method_with_default_value('December’)          #=> “It’s December in 2018"
method_with_default_value('December', 2019)    #=> “It’s December in 2019"
```

Ex3: the spat operator makes either all arguments optional ...

``` ruby
def any_num_of_params(*args)
    args
end

any_num_of_params(1, 2, 3, 4) #=> [1, 2, 3, 4]
```

Ex4: or, a sub-set of the parameters optional. Some are required, the rest are coerced into an array

```ruby
def any_num_of_params(required_arg, *optional_arg)
  p required_arg
  p optional_arg
end

any_num_of_params(1, 2, 3, 4) #=> 1 [2, 3, 4]
```

Ex5: an optional parameters situation, where changing the parameter list of a superclass won’t affect those in the subclass anymore. The method accepts any arguments but does nothing with them. A practical use would be in combo with the super keyword.

```ruby
class A
  def method_across_classes(param1, param2)
    puts param1
    puts param2
  end
end

class B < A
  def method_across_classes(*)
    super
  end
end

subclass_object = B.new
subclass_object.method_across_classes('param 1', 'param 2’) #=> param 1 param 2
```

Ex6: when types of arguments are mixed and matched, look at the progression of this example.

We intersperse method parameters that are required, optional, or default. And in the method invocation, we provide a variety of arguments that satisfies at least the “required” number of arguments.

Note the categories of parameters here: 1. required -> a, b, d, 2. optional: *c, \**d

*c turns rest of input into an array, while \**d catches the hashes

```ruby
def testing(a, b = 1, *c, d: 1, **x)
  p a,b,c,d,x
end

# case 1
testing(‘a’)
#=> ‘a' 1 [] 1 {} 
# both b and d return their default values, a is passed in, and there’s no optional values for c or x to collect

# case 2
testing('a', 'b', 'c')
#=> ‘a' ‘b’ [‘c’] 1 {} 
 # a b, d are the required values and both are passed in; d takes on the default value, and the argument ‘c’ is collected by *c

# case 3
testing('a', 'b', 'c', d: 2, x: 1)
#=> ‘a' ‘b’ [‘c’, ‘d'] 2 {: x= >1}
# note the syntax for parameter are method(required, default, optional arrays, default, optional hashes)
# switch the hashes and the arrays

# case 4
change the method definition slightly to:

def testing(a, b = 1, *c, d, **x)
  p a,b,c,d,x
end

testing('a', 'b', 'c', 'e', 'f', 'g', d, x: 1)
#=> ‘a' ‘b’ [‘c’, ‘e’, ‘f’] ‘g’ {:d => 2, : x= >1}
# my interpretation is that the *c is collecting all the optional value arguments passed in, until it meets the optional value arguments gathered by **x, then the string ‘g’ is passed in as the required parameter d

# case 5
# again a slight change to the method definition 

def testing(a, b = 1, d = 1, *c, **x)
  p a, b, d, c, x
end

testing('a', 'b', ‘c', 'e', 'f', 'g', d:2, x:1)
#=> ‘a' ‘b’, ‘c’, [‘e’, ‘f’, ‘g’] {:d => 2, : x= >1}
# in this case, a passes in a required parameter, b and d pass in optional value parameters (both have default values assigned), *c collects array [e, f, g], and the hash collects the rest

# case 6a
# what if we define the default value parameter after the optional value parameter?

def testing(a, b = 1, *c, d = 1, **x)
  p a, b, d, c, x
end

testing('a', 'b', ‘c', 'e', 'f', 'g', d:2, x:1)
#=> syntax error, unexpected '=', expecting ')'
# default value parameter must occur before optional value parameter

# case 6b
# unless the default value points to a hash

def testing(a, b = 1, *c, d: 1, **x)
  p a, b, d, c, x
end

testing('a', 'b', 'c', 'e', 'f', 'g', d: 2, x: 1)
#=> ‘a' ‘b’, 2, [‘c’, ‘e’, ‘f’, ‘g’] {: x= >1}
```
