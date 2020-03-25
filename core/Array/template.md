## Array

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

### Public Class Methods

#### ::new

```
Array.new → new_array
Array.new(array) → new_array
Array.new(size) → new_array
Array.new(size, default) → new_array
Array.new(size) { |index| ... } → new_array
```

With no block and no arguments, returns a new empty Array object:

```ruby
a = Array.new
a # => []
```

With no block and a single [Array-convertible object](#array-convertible-objects) argument,
converts the object and returns the resulting Array:

```ruby
Array.new([:foo, 'bar', baz = 2]) # => [:foo, "bar", 2]
```

With no block and a single [Integer-convertible object](#integer-convertible-objects) argument <tt>size</tt>,
returns a new Array object of the given size
whose elements are all <tt>nil</tt>:

```ruby
a = Array.new(0)
a # => []
a = Array.new(3)
a # => [nil, nil, nil]
```

With no block and two arguments,
an [Integer-convertible object](#integer-convertible-objects)  <tt>size</tt>
and another object <tt>obj</tt>,
returns an Array object of the given size
eac of whose elements is that same <tt>obj</tt>:

```ruby
a = Array.new(3, 'x') 
a # => ["x", "x", "x"]
a[0].object_id == a[1].object_id # => true
a[1].object_id == a[2].object_id # => true
```

With a block and a single [Integer-convertible object](#integer-convertible-objects) argument <tt>size</tt>,
returns an Array object of the given size;
the block is called with each successive integer <tt>n</tt>,
and each <tt>n</tt>th element is the return value from the block:

```ruby
a = Array.new(3) { |n| "Element #{n}" }
a # => ["Element 0", "Element 1", "Element 2"]
```

With a block and no argument,
or a single [Integer-convertible object](#integer-convertible-objects) argument that converts to <tt>0</tt>,
ignores the block and returns a new empty Array:

```ruby
a = Array.new(0) { |n| fail 'Cannot happen' }
a # => []
a = Array.new { |n| fail 'Cannot happen' }
a # => []
```

Raises an exception if <tt>size</tt> is a negative integer:

```ruby
Array.new(-1) # Raises ArgumentError (negative array size)
Array.new(-1, :default) # Raises ArgumentError (negative array size)
Array.new(-1) { |n| } # Raises ArgumentError (negative array size)
```

Raises an exception if the single argument
is neither an Array nor an Integer:

```ruby
Array.new(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### ::try_convert

```
Array.try_convert(obj) → new_array or nil
```

Returns the Array object created by calling
<tt>obj.to_ary</tt>.

```ruby
class ArrayableSet < Set
  def to_ary
    a = []
    self.map { |item| a.push(item) }
    a
  end
end
as = ArrayableSet.new([:foo, :bar, :baz])
Array.try_convert(as) # => [:foo, :bar, :baz]
```

Returns <tt>nil</tt> unless <tt>obj.respond_to?(:to_ary)</tt>:

```ruby
s = 'foo'
s.respond_to?(:to_ary) # => false
Array.try_convert(s) # => nil
```

Raises an exception unless <tt>obj.to_ary</tt> returns an Array object:

```ruby
class BadToArySet < Set
  def to_ary
    1
  end
end
hs = BadToArySet.new([:foo, :bar, :baz])
Array.try_convert(hs) # Raises TypeError (can't convert BadToArySet to Array (BadToArySet#to_ary gives Integer))
```

### Public Instance Methods

#### << (Append)

```
ary << obj → self
```

Appends <tt>obj</tt> to <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a << :bam
a1 # => [:foo, "bar", 2, :bam]
a1.object_id == a.object_id # => true
```

Chained:

```ruby
a = [:foo, 'bar', baz = 2]
a << :bam << :bat
a # => [:foo, "bar", 2, :bam, :bat]
```

#### [] (Element Reference)

```
ary[n] → obj or nil
ary[start, length] → new_array or nil
ary[range] → new_array or nil
```

Returns elements from <tt>ary</tt>; does not remove the elements from <tt>ary</tt>.

When the only argument is an Integer <tt>n</tt>,
returns the <tt>n</tt>th element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0] # => :foo
a[2] # => 2
a # => [:foo, "bar", 2]
```

If <tt>n</tt> is negative,
counts relative to the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[-1] # => 2
a[-2] # => "bar"
```

If <tt>n</tt> is out of range,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[50] # => nil
a[-50] # => nil
```

When the only arguments are Integers <tt>start</tt> and <tt>length</tt>,
returns a new array of size <tt>length</tt>
containing elements of <tt>ary</tt> beginning at index <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0, 2] # => [:foo, "bar"]
a[1, 2] # => ["bar", 2]
```

If <tt>start + length</tt> is greater than <tt>ary.length</tt>,
returns <tt>ary.size - start</tt> elements:

```ruby
a = [:foo, 'bar', baz = 2]
a[0, 50] # => [:foo, "bar", 2]
a[1, 50] # => ["bar", 2]
```

If <tt>start == a.size</tt> and <tt>length >= 0</tt>,
returns a new empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a[a.size, 0] # => []
a[a.size, 50] # => []
```

If <tt>length</tt> is negative,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[2, -1] # => nil
a[1, -2] # => nil
```

When the only argument is a Range object <rng>,
treats <tt>rng.min</tt> as <tt>start</tt> above
and <tt>rng.size</tt> as <tt>length</tt> above:

```ruby
a = [:foo, 'bar', baz = 2]
a[0..1] # => [:foo, "bar"]
a[1..2] # => ["bar", 2]
```

If <tt>rng.start == a.size</tt>,
returns a new empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a[a.size..0] # => []
a[a.size..50] # => []
a[a.size..-1] # => []
a[a.size..-50] # => []
```

If <tt>rng.end</tt> is negative,
calculates the end index from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0..-1] # => [:foo, "bar", 2]
a[0..-2] # => [:foo, "bar"]
a[0..-3] # => [:foo]
a[0..-4] # => []
```

If <tt>rng.start</tt> is negative,
calculates the start index from the end of <tt>ary</tt>:
```ruby
a = [:foo, 'bar', baz = 2]
a[-1..2] # => [2]
a[-2..2] # => ["bar", 2]
a[-3..2] # => [:foo, "bar", 2]
```

Raises an exception if given a single argument that is not an Integer:

```ruby
a = [:foo, 'bar', baz = 2]
a[:foo] # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if given two arguments that are not both Integers:

```ruby
a = [:foo, 'bar', baz = 2]
a[:foo, 3] # Raises TypeError (no implicit conversion of Symbol into Integer)
a[1, :bar] # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### append

```
ary.append(*objects) → self
```

Appends the given <tt>*objects</tt> to <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.append(:bam, :bat)
a1 # => [:foo, "bar", 2, :bam, :bat]
a1.object_id == a.object_id # => true
```

Chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.append(:bam, :bat).append(:bad, :bah)
a # => [:foo, "bar", 2, :bam, :bat, :bad, :bah]
```

#### at

```
ary.at(index) → obj
```

Returns the element at <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.at(0) # => :foo
a.at(2) # => 2
```

#### freeze

```
ary.freeze → self
```

Freezes <tt>self</tt>, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.frozen? # => false
a1 = a.freeze 
a1.object_id == a.object_id # => true
```

Chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.freeze.frozen? # => true
```

Raises an exception for an attempt to modify a frozen Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.freeze
a[3] = :bat # Raises FrozenError (can't modify frozen Array: [:foo, "bar", 2])
```

#### pop

```
ary.pop → obj or nil
ary.pop(n) → new_array
```
Removes and returns trailing elements in <tt>ary</tt>.

If no argument is given and <tt>ary</tt> is empty,
returns <tt>nil</tt>:

```ruby
a = []
a.pop # => nil
```

If no argument is given and <tt>ary</tt> is not empty,
removes and returns the last element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.pop # => 2
a # => [:foo, "bar"]
```

If an Integer argument <tt>n</tt> given and <tt>ary</tt> is empty,
returns a new empty Array:

```ruby
a = []
a.pop(1) # => []
a.pop(2) # => []
```

If an Integer argument <tt>n</tt> is given and <tt>ary</tt> is not empty,
removes and returns the last <tt>n</tt> elements in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.pop(2)
a1 # => ["bar", 2]
a # => [:foo]
```

If <tt>n</tt> is greater than <tt>ary.size</tt>,
removes and returns all elements in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.pop(50)
a1 # => [:foo, "bar", 2]
a # => []
```

Returns a new empty Array if <tt>n</tt> is <tt>0</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.pop(0)
a1 # => []
a # => [:foo, "bar", 2]
```

Raises an exception if <tt>n</tt> is negative:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.pop(-1) # Raises ArgumentError (negative array size)
```

#### prepend

```
ary.prepend(*objects) → self
```

Prepends the given <tt>*objects</tt> to <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.prepend(:bam, :bat)
a1 # => [:bam, :bat, :foo, "bar", 2]
a1.object_id == a.object_id # => true
```

Chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.prepend(:bam, :bat).prepend(:bad, :bah)
a # => [:bad, :bah, :bam, :bat, :foo, "bar", 2]
```

#### push

```
ary.push(*objects) → self
```

Appends the given <tt>*objects</tt> to <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.push(:bam, :bat)
a1 # => [:foo, "bar", 2, :bam, :bat]
a1.object_id == a.object_id # => true
```

Chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.push(:bam, :bat).push(:bad, :bah)
a # => [:foo, "bar", 2, :bam, :bat, :bad, :bah]
```

#### shift

```
ary.shift → obj or nil
ary.shift(n) → new_array
```

Removes and returns leading elements in <tt>ary</tt>.

If no argument is given and <tt>ary</tt> is empty
returns <tt>nil</tt>:

```ruby
a = []
a.shift # => nil
```

If no argument is given and <tt>ary</tt> is not empty,
removes and returns the first element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.shift # => :foo
a # => ["bar", 2]
```

If an Integer argument <tt>n</tt> given and <tt>ary</tt> is empty,
returns a new empty Array:

```ruby
a = []
a.shift(1) # => []
a.shift(2) # => []
```

If an Integer argument <tt>n</tt> is given and <tt>ary</tt> is not empty,
removes and returns the first <tt>n</tt> elements in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.shift(2)
a1 # => [:foo, "bar"]
a # => [2]
```

If <tt>n</tt> is greater than <tt>ary.size</tt>,
removes and returns all elements in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.shift(50)
a1 # => [:foo, "bar", 2]
a # => []
```

Returns a new empty Array if <tt>n</tt> is <tt>0</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.shift(0)
a1 # => []
a # => [:foo, "bar", 2]
```

Raises an exception if <tt>n</tt> is negative:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.shift(-1) # Raises ArgumentError (negative array size)
```

#### slice

```
ary.slice(n) → obj or nil
ary.slice(start, length) → new_array or nil
ary.slice(range) → new_array or nil
```

Returns elements from <tt>ary</tt>; does not remove the elements from <tt>ary</tt>.

When the only argument is an Integer <tt>n</tt>,
returns the <tt>n</tt>th element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(0) # => :foo
a.slice(2) # => 2
a # => [:foo, "bar", 2]
```

If <tt>n</tt> is negative,
counts relative to the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(-1) # => 2
a.slice(-2) # => "bar"
```

If <tt>n</tt> is out of range,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(50) # => nil
a.slice(-50) # => nil
```

When the only arguments are Integers <tt>start</tt> and <tt>length</tt>,
returns a new array of size <tt>length</tt>
containing elements of <tt>ary</tt> beginning at index <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(0, 2) # => [:foo, "bar"]
a.slice(1, 2) # => ["bar", 2]
```

If <tt>start + length</tt> is greater than <tt>ary.length</tt>,
returns <tt>ary.size - start</tt> elements:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(0, 50) # => [:foo, "bar", 2]
a.slice(1, 50) # => ["bar", 2]
```

If <tt>start == a.size</tt> and <tt>length >= 0</tt>,
returns a new empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(a.size, 0) # => []
a.slice(a.size, 50) # => []
```

If <tt>length</tt> is negative,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(2, -1) # => nil
a.slice(1, -2) # => nil
```

When the only argument is a Range object <rng>,
treats <tt>rng.min</tt> as <tt>start</tt> above
and <tt>rng.size</tt> as <tt>length</tt> above:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(0..1) # => [:foo, "bar"]
a.slice(1..2) # => ["bar", 2]
```

If <tt>rng.start == a.size</tt>,
returns a new empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(a.size..0) # => []
a.slice(a.size..50) # => []
a.slice(a.size..-1) # => []
a.slice(a.size..-50) # => []
```

If <tt>rng.end</tt> is negative,
calculates the end index from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(0..-1) # => [:foo, "bar", 2] 
a.slice(0..-2) # => [:foo, "bar"]
a.slice(0..-3) # => [:foo]
a.slice(0..-4) # => []
```

If <tt>rng.start</tt> is negative,
calculates the start index from the end of <tt>ary</tt>:
```ruby
a = [:foo, 'bar', baz = 2]
a.slice(-1..2) # => [2] 
a.slice(-2..2) # => ["bar", 2]
a.slice(-3..2) # => [:foo, "bar", 2]
```

Raises an exception if given a single argument that is not an Integer:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if given two arguments that are not both Integers:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(:foo, 3) # Raises TypeError (no implicit conversion of Symbol into Integer)
a.slice(1, :bar) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### unshift

```
ary.unshift(*objects) → self
```

Prepends the given <tt>*objects</tt> to <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.unshift(:bam, :bat)
a1 # => [:bam, :bat, :foo, "bar", 2]
a1.object_id == a.object_id # => true
```

Chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.unshift(:bam, :bat).unshift(:bad, :bah)
a # => [:bad, :bah, :bam, :bat, :foo, "bar", 2]
```

