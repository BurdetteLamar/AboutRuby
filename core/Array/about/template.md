## About Array

An Array is an ordered collection of elements.
* An Array element can be any object.
* The elements within an Array may be objects of different classes.

An Array element can be accessed via an Integer index:
* Access the first element with index <tt>0</tt>,
  the second with index <tt>1</tt>, ....
* Access the last element with index <tt>-1</tt>,
  the next-to-last with index <tt>-2</tt>, .... 

@[:page_toc](### Contents)

### Creating an Array

Here are four ways to create an Array:
* Constructor method: <tt>Array.new.</tt>
* Literal method: <tt>Array[]</tt>.
* Implicit Form: <tt>[]</tt>.
* Kernel method: <tt>Array(*elements)</tt>

#### Constructor Array.new

You can create an Array using the constructor method, <tt>Array.new</tt>.

Create an empty Array:

```ruby
a = Array.new
a # => []
a.class # => Array
```

To create an Array of a given size, pass an Integer argument:

```ruby
a = Array.new(3)
a # => [nil, nil, nil]
```

#### Array Literal

You can create an Array using its literal method, <tt>Array[]</tt>.

Create an empty Array:

```ruby
a = Array[]
a # => []
```

Create an Array with initial elements:

```ruby
a = Array[:foo, 'bar', baz = 2]
a # => [:foo, "bar", 2]
```

#### Array Implicit Form

You can create an Array using its implicit form (square brackets).

Create an empty Array:

```ruby
a = []
a # => []
```

Create an Array with initial elements:

```ruby
a = [:foo, 'bar', baz = 2]
a # => [:foo, "bar", 2]
```

#### Kernel Method Array

You can create an Array using Kernel method <tt>Array</tt>.
See Kernel#Array.

### Array Element Basics

#### Getting an Array element:

The simplest way to get an Array value (instance method <tt>[]</tt>):

```ruby
a = [:foo, 'bar', baz = 2]
a[0] # => :foo
```

#### Setting an Array Element

The simplest way to create or update an Array element (instance method <tt>[]=</tt>):

```ruby
a = [:foo, 'bar', baz = 2]
a[0] = :bat
a # => [:bat, "bar", 2]
a[3] = 'bam'
a # => [:bat, "bar", 2, "bam"]
```

#### Deleting an Array Element

The simplest way to delete an Array element (instance method <tt>delete</tt>):

```ruby
a = [:foo, 'bar', baz = 2]
a.delete('bar') 
a # => [:foo, 2]
```

### Chaining Method Calls

Many Array instance methods return <tt>self</tt>.
For those methods, you can chain method calls:

```ruby
a = [:foo, 'bar', baz = 2]
a.freeze.frozen? # => true
```

### Methods Returning Enumerators

Each Array method that allows a block for iteration
returns an Enumerator if no block is given.

These are:
* [a.each](../api/markdown.md#each)
* [a.each_index](../api/markdown.md#each_index)
* [a.reverse_each](../api/markdown.md#reverse_each)
* [a.find_index](../api/markdown.md#find_index)
* [a.index](../api/markdown.md#index)
* [a.rindex](../api/markdown.md#rindex)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)
* [a.](../api/markdown.md#)


Example using method <tt>ary.each_index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
e = a.each_index
e # => #<Enumerator: [:foo, "bar", 2]:each_index>
```

And perhaps later on:

a1 = e.each { |index|  puts "#{index} #{a[index]}"}
a1.object_id == a.object_id # => true
```

Output:

```
0 foo
1 bar
2 2
```

The enumerator has a _reference to_ the Array, not a copy of it:

```ruby
e # => #<Enumerator: [:foo, "bar", 2]:each_index>
a[0] = 'foo'
e # => #<Enumerator: ["foo", "bar", 2]:each_index>
```

But making the original Hash unavailable causes the enumerator to make a copy:

```ruby
a = nil
e # => #<Enumerator: ["foo", "bar", 2]:each_index>
a = []
e # => #<Enumerator: ["foo", "bar", 2]:each_index>
```
