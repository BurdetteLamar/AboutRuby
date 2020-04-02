## Array API

@[:page_toc](### Contents)

### Public Class Methods

#### ::new

```
Array.new → new_empty_array
Array.new(array) → new_array
Array.new(size) → new_array
Array.new(size, default_value) → new_array
Array.new(size) { |index| ... } → new_array
```

Returns a new Array.

Argument <tt>array</tt>, if given, must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to an Array.

Argument <tt>size</tt>, if given must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

Argument <tt>default_value</tt> may be any object.

---

With no block and no arguments, returns a new empty Array object:

```ruby
a = Array.new
a # => []
```

---

With no block and a single argument <tt>array</tt>,
returns a new Array formed from <tt>array</tt>:

```ruby
a = Array.new([:foo, 'bar', baz = 2])
a.class # => Array
a # => [:foo, "bar", 2]
```

---

With no block and a single argument <tt>size</tt>,
returns a new Array of the given size
whose elements are all <tt>nil</tt>:

```ruby
a = Array.new(0)
a # => []
a = Array.new(3)
a # => [nil, nil, nil]
```

---

With no block and arguments <tt>size</tt> and  <tt>default_value</tt>,
returns an Array of the given size;
each element is that same <tt>default_value</tt>:

```ruby
a = Array.new(3, 'x') 
a # => ["x", "x", "x"]
a[0].object_id == a[1].object_id # => true
a[1].object_id == a[2].object_id # => true
```

---

With a block and a argument <tt>size</tt>,
returns an Array of the given size;
the block is called with each successive integer <tt>index</tt>;
the element for that <tt>index</tt> is the return value from the block:

```ruby
a = Array.new(3) { |index| "Element #{index}" }
a # => ["Element 0", "Element 1", "Element 2"]
```

---

With a block and no argument,
or a single argument <tt>0</tt>,
ignores the block and returns a new empty Array:

```ruby
a = Array.new(0) { |n| fail 'Cannot happen' }
a # => []
a = Array.new { |n| fail 'Cannot happen' }
a # => []
```

---

With a block and both <tt>size</tt> and <tt>default_value</tt>,
gives a warning message
(<tt>warning: block supersedes default value argument</tt>),
and assigns elements from the block's return values:

```ruby
Array.new(4, :default) {} # => [nil, nil, nil, nil]
```

---

Raises an exception if <tt>size</tt> is a negative integer:

```ruby
Array.new(-1) # Raises ArgumentError (negative array size)
Array.new(-1, :default) # Raises ArgumentError (negative array size)
Array.new(-1) { |n| } # Raises ArgumentError (negative array size)
```

Raises an exception if the single argument is neither
an [Array-convertible object](../../../doc/convertibles.md#array-convertible-objects) nor
an [Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
Array.new(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### ::try_convert

```
Array.try_convert(obj) → new_array or nil
```

Tries to convert <tt>obj</tt> to an Array.

---

When <tt>obj</tt> is
an [Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
returns the Array object created by converting it:

```ruby
class ToAryReturnsArray < Set
  def to_ary
    self.to_a
  end
end
as = ToAryReturnsArray.new([:foo, :bar, :baz])
Array.try_convert(as) # => [:foo, :bar, :baz]
```

---

Returns <tt>nil</tt> unless <tt>obj.respond_to?(:to_ary)</tt>:

```ruby
s = 'foo'
s.respond_to?(:to_ary) # => false
Array.try_convert(s) # => nil
```

---

Raises an exception if <tt>obj.to_ary</tt> takes an argument:

```ruby
class ToAryTakesArgument
  def to_ary(x)
    []
  end
end
Array.try_convert(ToAryTakesArgument.new) # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

Raises an exception if <tt>obj.to_ary</tt> returns a non-Array:

```ruby
class ToAryTakesArgument
  def to_ary
    :foo
  end
end
Array.try_convert(ToAryTakesArgument.new) # Raises TypeError (can't convert ToAryTakesArgument to Array (ToAryTakesArgument#to_ary gives Symbol))
```

### Public Instance Methods

#### << (Append)

```
ary << obj → self
```

Appends <tt>obj</tt> to <tt>ary</tt>;
returns <tt>ary</tt> (<tt>self</tt>):

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a << :bam
a1 # => [:foo, "bar", 2, :bam]
a1.object_id == a.object_id # => true
```

May be chained:

```ruby
a = [:foo, 'bar', baz = 2]
a << :bam << :bat
a # => [:foo, "bar", 2, :bam, :bat]
```

#### [] (Element Reference)

```
ary[index] → obj or nil
ary[start, length] → obj or nil
ary[range] → obj or nil
```

Returns elements from <tt>ary</tt>.

Arguments <tt>index</tt>, <tt>start</tt>, and <tt>length</tt>, if given, must be
[Integer-convertible objects](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to Integers.

Argument <tt>range</tt>, if given, must be a Range object.

---

When a single argument <tt>index</tt> is given,
returns the element in <tt>ary</tt> at offset <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0] # => :foo
a[2] # => 2
a # => [:foo, "bar", 2]
```

If <tt>index</tt> is negative,
counts relative to the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[-1] # => 2
a[-2] # => "bar"
```

If <tt>index</tt> is out of range,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[50] # => nil
a[-50] # => nil
```

---

When two arguments <tt>start</tt> and <tt>length</tt> are given,
returns a new array of size <tt>length</tt>
containing successive elements of <tt>ary</tt> beginning at offset <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0, 2] # => [:foo, "bar"]
a[1, 2] # => ["bar", 2]
```

If <tt>start + length > ary.length</tt>,
returns <tt>ary.size - start</tt> elements:

```ruby
a = [:foo, 'bar', baz = 2]
a[0, 4] # => [:foo, "bar", 2]
a[1, 3] # => ["bar", 2]
a[2, 2] # => [2]
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

---

When a single argument <tt>range</tt> is given,
treats <tt>rng.min</tt> as <tt>start</tt> above
and <tt>rng.size</tt> as <tt>length</tt> above:

```ruby
a = [:foo, 'bar', baz = 2]
a[0..1] # => [:foo, "bar"]
a[1..2] # => ["bar", 2]
```

Special case: If <tt>rng.start == a.size</tt>,
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

---

Raises an exception if given a single argument that is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects)
or a Range object:

```ruby
a = [:foo, 'bar', baz = 2]
a[:foo] # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if given two arguments that are not both
[Integer-convertible objects](../../../doc/convertibles.md#integer-convertible-objects)

```ruby
a = [:foo, 'bar', baz = 2]
a[:foo, 3] # Raises TypeError (no implicit conversion of Symbol into Integer)
a[1, :bar] # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### []= (Element Assignment)

```
ary[index] = obj -> obj
ary[start, length] = obj ->  obj
ary[range] = obj ->  obj
```

Assigns elements in <tt>ary</tt>; returns the given <tt>obj</tt>.

Arguments <tt>index</tt>, <tt>start</tt>, and <tt>length</tt>, if given, must be
[Integer-convertible objects](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to Integers.

Argument <tt>range</tt>, if given, must be a Range object.

If <tt>obj</tt> is an
[Array-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
it will be converted to an Array.

---

When <tt>index</tt> is given, assigns <tt>obj</tt>
to an element in <tt>ary</tt>.

If <tt>index</tt> is non-negative,
assigns <tt>obj</tt> the element at offset <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0] = 'foo' # => "foo"
a # => ["foo", "bar", 2]
```

If <tt>index >= ary.length</tt>, extends <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[7] = 'foo' # => "foo"
a # => [:foo, "bar", 2, nil, nil, nil, nil, "foo"]
```

If <tt>index</tt> is negative, counts backward from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[-1] = 'two' # => "two"
a # => [:foo, "bar", "two"]
```

---

When <tt>start</tt> and <tt>length</tt> are given
and <tt>obj</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
removes <tt>length - 1</tt> elements beginning at offset <tt>start</tt>,
and assigns <tt>obj</tt> at offset <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0, 2] = 'foo' # => "foo"
a # => ["foo", 2]
```

if <tt>start</tt> is negative, counts backward from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[-2, 2] = 'foo' # => "foo"
a # => [:foo, "foo"]
```

If <tt>start >= ary.size</tt>,
extends <tt>ary</tt> with <tt>nil</tt>, assigns <tt>ary[start] = obj</tt>,
and ignores <tt>length</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[6, 50] = 'foo' # => "foo"
a # => [:foo, "bar", 2, nil, nil, nil, "foo"]
```

If <tt>length == 0</tt>, shifts elements at and following offset <tt>start</tt>
and assigns <tt>obj</tt> at offset <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[1, 0] = 'foo' # => "foo"
a # => [:foo, "foo", "bar", 2]
```

If <tt>length</tt> is too large for the existing <tt>ary</tt>,
does not extend <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[1, 5] = 'foo' # => "foo"
a # => [:foo, "foo"]
```

---

When <tt>range</tt> is given and <tt>obj</tt> is an
[Array-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
removes <tt>length - 1</tt> elements beginning at offset <tt>start</tt>,
and assigns <tt>obj</tt> at offset <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[0..1] = 'foo' # => "foo"
a # => ["foo", 2]
```

if <tt>range.begin</tt> is negative, counts backward from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[-2..2] = 'foo' # => "foo"
a # => [:foo, "foo"]
```

If <tt>range.begin >= ary.size</tt>,
extends <tt>ary</tt> with <tt>nil</tt>, assigns <tt>ary[range.begin] = obj</tt>,
and ignores <tt>length</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[6..50] = 'foo' # => "foo"
a # => [:foo, "bar", 2, nil, nil, nil, "foo"]
```

If <tt>range.end == 0</tt>, shifts elements at and following offset <tt>start</tt>
and assigns <tt>obj</tt> at offset <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[1..0] = 'foo' # => "foo"
a # => [:foo, "foo", "bar", 2]
```

If <tt>range.end</tt> is negative,
assigns <tt>obj</tt> at offset <tt>start</tt>,
retains <tt>range.end.abs -1</tt> elements past that,
and removes those beyond:

```ruby
a = [:foo, 'bar', baz = 2]
a[1..-1] = 'foo' # => "foo"
a # => [:foo, "foo"]
a = [:foo, 'bar', baz = 2]
a[1..-2] = 'foo' # => "foo"
a # => [:foo, "foo", 2]
a = [:foo, 'bar', baz = 2]
a[1..-3] = 'foo' # => "foo"
a # => [:foo, "foo", "bar", 2]
a = [:foo, 'bar', baz = 2]
```

If <tt>range.end</tt> is too large for the existing <tt>ary</tt>,
does not extend <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[1..5] = 'foo' # => "foo"
a # => [:foo, "foo"]
```

---

Raises an exception if given a single argument that is not
an [Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects)
or a Range:

```ruby
a = [:foo, 'bar', baz = 2]
a[:nosuch] = 'two' # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if given two arguments that are not both
[Integer-convertible objects](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a[:nosuch, 2] = 'two' # Raises TypeError (no implicit conversion of Symbol into Integer)
a[0, :nosuch] = 'two' # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if a negative <tt>index</tt> is out of range:

```ruby
a = [:foo, 'bar', baz = 2]
a[-4] = 'two' # Raises IndexError (index -4 too small for array; minimum: -3)
```

Raises an exception if <tt>start</tt> is too small for <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a[-5, 2] = 'foo' # Raises IndexError (index -5 too small for array; minimum: -3)
```

Raises an exception if <tt>length</tt> is negative:

```ruby
a = [:foo, 'bar', baz = 2]
a[1, -1] = 'foo' # Raises IndexError (negative length (-1))
```

#### append

```
ary.append(*objects) → self
```

Appends the given <tt>*objects</tt> to <tt>ary</tt>; returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.append(:bam, :bat)
a1 # => [:foo, "bar", 2, :bam, :bat]
a1.object_id == a.object_id # => true
```

May be chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.append(:bam, :bat).append(:bad, :bah)
a # => [:foo, "bar", 2, :bam, :bat, :bad, :bah]
```

#### at

```
ary.at(index) → obj
```

Returns the element in <tt>ary</tt> at offset <tt>index</tt>.

Argument <tt>index</tt> must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

---

Returns the element at offset <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.at(0) # => :foo
a.at(2) # => 2
```

---

Raises an exception if <tt>index</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a.at(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### each

```
ary.each { |element| ... } -> self
ary.each -> Enumerator
```

When a block given,
passes each successive <tt>element</tt> in <tt>ary</tt>
to the block, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.each { |element|  puts "#{element.class} #{element}" }
a1.object_id == a.object_id # => true
```

Output:

```
Symbol foo
String bar
Integer 2
```

Allows <tt>ary</tt> to be modified during the iteration:

```ruby
a = [:foo, 'bar', baz = 2]
a.each { puts element; a.clear if element.to_s.start_with?('b') }
a # => []
```

Output:

```
foo
bar
```

---

When no block given, returns a new Enumerator:

```ruby
a = [:foo, 'bar', baz = 2]
e = a.each
e # => #<Enumerator: [:foo, "bar", 2]:each>
a1 = e.each { |element|  puts "#{element.class} #{element}" }
a1.object_id == a.object_id # => true
```

Output:

```
Symbol foo
String bar
Integer 2
```

#### each_index

```
ary.each_index { |index| ... } -> self
ary.each_index -> Enumerator
```

When a block given,
passes each successive <tt>index</tt> in <tt>ary</tt>
to the block, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.each_index { |index|  puts "#{index} #{a[index]}" }
a1.object_id == a.object_id # => true
```

Output:

```
0 foo
1 bar
2 2
```

Allows <tt>ary</tt> to be modified during the iteration:

```ruby
a = [:foo, 'bar', baz = 2]
a.each_index { |index| puts index; a.clear if index > 0 }
a # => []
```

Output:

```
0
1
```

---

When no block given, returns a new Enumerator:

```ruby
a = [:foo, 'bar', baz = 2]
e = a.each_index
e # => #<Enumerator: [:foo, "bar", 2]:each_index>
a1 = e.each { |index|  puts "#{index} #{a[index]}"}
a1.object_id == a.object_id # => true
```

Output:

```
0 foo
1 bar
2 2
```

#### fetch

```
ary.fetch(index) -> obj
ary.fetch(index, default_value) -> obj
ary.fetch(index) { |index| ... } -> obj
```

Returns the element at index <tt>index</tt>.

The given <tt>index</tt> must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

With the single argument <tt>index</tt>,
returns <tt>ary[index]</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch(1) # => "bar"
```

If <tt>index < 0</tt>, counts from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch(-1) # => 2
```

---

With arguments <tt>index</tt> and <tt>default_value</tt>,
returns <tt>ary[index]</tt> if <tt>index</tt> is in range,
otherwise <tt>default_value</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch(1, nil) # => "bar"
a.fetch(50, nil) # => nil
```

---

With argument <tt>index</tt> and a block,
returns <tt>ary[index]</tt> if <tt>index</tt> is in range
(and the block is not called),
otherwise calls the block with <tt>index</tt> and returns its return value:

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch(1) { |index| fail 'Cannot happen' } # => "bar"
a.fetch(50) { |index| "Value for #{index}" } # => "Value for 50"
```

---

Raises an exception if <tt>index</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch('x') # Raises TypeError (no implicit conversion of String into Integer)
```

Raises an exception if <tt>index</tt> is out of range
and neither <tt>default_value</tt> nor a block given:

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch(50) # Raises IndexError (index 50 outside of array bounds: -3...3)
```

#### find_index

```
ary.find_index(obj) -> Integer or nil
ary.find_index { |element| ... } -> Integer or nil
ary.find_index -> new_enumerator
```

---

When argument <tt>obj</tt> is given,
returns the index of the first element <tt>element</tt>
for which <tt>obj == element</tt>:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.find_index('bar') # => 1
```

Returns <tt>nil</tt> if no such object found:

```ruby
a = [:foo, 'bar', baz = 2]
a.find_index(:nosuch) # => nil
```

---

When a block is given,
calls the block with each successive element <tt>element</tt>,
returning the index of the first <tt>element</tt>
for which the block returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.find_index { |element| element == 'bar' } # => 1
```

Returns <tt>nil</tt> if the block never returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2]
a.find_index { |element| element == 'x' } # => nil
```

---

When neither an argument nor a block is given,
returns a new Enumerator:

```ruby
a = [:foo, 'bar', baz = 2]
e = a.find_index
e # => #<Enumerator: [:foo, "bar", 2]:find_index>
e.each { |element| element == 'bar' } # => 1
```

---

When both an argument and a block given,
gives a warning (<tt>warning: given block not used</tt>)
and ignores the block:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
index = a.find_index('bar') { fail 'Cannot happen' }
index # => 1
```
#### first

```
ary.first → obj or nil
ary.first(n) → new_array
```

Returns elements from <tt>ary</tt>.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

---

When no argument is given, returns the first element:

```ruby
a = [:foo, 'bar', baz = 2]
a.first # => :foo
a # => [:foo, "bar", 2]
```

If <tt>ary</tt>> is empty, returns <tt>nil</tt>:

```ruby
[].first # => nil
```

---

When argument <tt>n</tt> is given,
returns the first <tt>n</tt> elements in a new Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.first(2) # => [:foo, "bar"]
```

When <tt>n >= ary.size</tt>, returns all elements:

```ruby
a = [:foo, 'bar', baz = 2]
a.first(50) # => [:foo, "bar", 2]
```

When <tt>n == 0</tt>, returns an empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.first(0) # []
```

---

Raises an exception if <tt>n < 0</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.first(-1) # Raises ArgumentError (negative array size)
```

Raises an exception if <tt>n</tt> is not
an [Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a.first('x') # Raises TypeError (no implicit conversion of String into Integer)
```

#### freeze

```
ary.freeze → self
```

Freezes <tt>ary</tt>, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.frozen? # => false
a1 = a.freeze 
a1.object_id == a.object_id # => true
```

May be chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.freeze.frozen? # => true
```

---

Raises an exception for an attempt to modify a frozen Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.freeze
a[3] = :bat # Raises FrozenError (can't modify frozen Array: [:foo, "bar", 2])
```

#### index

```
ary.index(obj) -> Integer or nil
ary.index { |element| ... } -> Integer or nil
ary.index -> new_enumerator
```

---

When argument <tt>obj</tt> is given,
returns the index of the first element <tt>element</tt>
for which <tt>obj == element</tt>:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.index('bar') # => 1
```

Returns <tt>nil</tt> if no such object found:

```ruby
a = [:foo, 'bar', baz = 2]
a.index(:nosuch) # => nil
```

---

When a block is given,
calls the block with each successive element <tt>element</tt>,
returning the index of the first <tt>element</tt>
for which the block returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.index { |element| element == 'bar' } # => 1
```

Returns <tt>nil</tt> if the block never returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2]
a.index { |element| element == 'x' } # => nil
```

---

When neither an argument nor a block is given,
returns a new Enumerator:

```ruby
a = [:foo, 'bar', baz = 2]
e = a.index
e # => #<Enumerator: [:foo, "bar", 2]:index>
e.each { |element| element == 'bar' } # => 1
```

---

When both an argument and a block given,
gives a warning (<tt>warning: given block not used</tt>)
and ignores the block:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
index = a.index('bar') { fail 'Cannot happen' }
index # => 1
```

#### insert

```
ary.insert(index, *objects) -> self
```

Inserts <tt>*objects</tt> before or after the element at offset <tt>index</tt>;
returns <tt>self</tt>.

Argument <tt>index</tt> must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

---

When <tt>index</tt> is non-negative, inserts <tt>*objects</tt>
before the element at offset <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.insert(1, :bat, :bam)
a # => [:foo, :bat, :bam, "bar", 2]
a1.object_id == a.object_id # => true
```

If <tt>index >= ary.length</tt>, extends <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.insert(5, :bat, :bam)
a # => [:foo, "bar", 2, nil, nil, :bat, :bam]
```

Does nothing if no <tt>*objects</tt> given:

```ruby
a = [:foo, 'bar', baz = 2]
a.insert(1)
a.insert(50)
a.insert(-50)
a # => [:foo, "bar", 2]
```

When <tt>index</tt> is negative,
inserts <tt>*objects</tt> after the element
at offset <tt>index + ary.length</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.insert(-2, :bat, :bam)
a # => [:foo, "bar", :bat, :bam, 2]
```

---

Raises an exception if <tt>index</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.insert(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if <tt>index</tt> is too small:

```ruby
a = [:foo, 'bar', baz = 2]
a.insert(-5, :bat, :bam) # Raises IndexError (index -5 too small for array; minimum: -4) 
```

#### last

```
ary.last → obj or nil
ary.last(n) → new_array
```

Returns elements from <tt>ary</tt>.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

---

When no argument is given, returns the last element:

```ruby
a = [:foo, 'bar', baz = 2]
a.last # => 2
a # => [:foo, "bar", 2]
```

If <tt>ary</tt> is empty, returns <tt>nil</tt>:

```ruby
[].last # => nil
```

---

When argument <tt>n</tt> is given,
returns the last <tt>n</tt> elements in a new Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.last(2) # => ["bar", 2]
```

When <tt>n >= ary.size</tt>, returns all elements:

```ruby
a = [:foo, 'bar', baz = 2]
a.last(50) # => [:foo, "bar", 2]
```

When <tt>n == 0</tt>, returns an empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.last(0) # []
```

---

Raises an exception if <tt>n < 0</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.last(-1) # Raises ArgumentError (negative array size)
```

Raises an exception if <tt>n</tt> is not
an [Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a.last('x') # Raises TypeError (no implicit conversion of String into Integer)
```

#### pop

```
ary.pop → obj or nil
ary.pop(n) → new_array
```

Removes and returns trailing elements from <tt>ary</tt>.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

---

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

---

If argument <tt>n</tt> is given and <tt>ary</tt> is empty,
returns a new empty Array:

```ruby
a = []
a.pop(1) # => []
a.pop(2) # => []
```

If argument <tt>n</tt> is given and <tt>ary</tt> is not empty,
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

---

Raises an exception if <tt>n</tt> is negative:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.pop(-1) # Raises ArgumentError (negative array size)
```

#### prepend

```
ary.prepend(*objects) → self
```

Prepends the given <tt>*objects</tt> to <tt>ary</tt>, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.prepend(:bam, :bat)
a1 # => [:bam, :bat, :foo, "bar", 2]
a1.object_id == a.object_id # => true
```

May be chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.prepend(:bam, :bat).prepend(:bad, :bah)
a # => [:bad, :bah, :bam, :bat, :foo, "bar", 2]
```

#### push

```
ary.push(*objects) → self
```

Appends the given <tt>*objects</tt> to <tt>ary</tt>; returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.push(:bam, :bat)
a1 # => [:foo, "bar", 2, :bam, :bat]
a1.object_id == a.object_id # => true
```

May be chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.push(:bam, :bat).push(:bad, :bah)
a # => [:foo, "bar", 2, :bam, :bat, :bad, :bah]
```

#### rindex

```
ary.rindex(obj) -> Integer or nil
ary.rindex { |element| ... } -> Integer or nil
ary.rindex -> new_enumerator
```

---

When argument <tt>obj</tt> is given,
returns the index of the last element <tt>element</tt>
for which <tt>obj == element</tt>:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.rindex('bar') # => 3
```

Returns <tt>nil</tt> if no such object found:

```ruby
a = [:foo, 'bar', baz = 2]
a.rindex(:nosuch) # => nil
```

---

When a block is given,
calls the block with each successive element <tt>element</tt>,
returning the index of the last <tt>element</tt>
for which the block returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a.rindex { |element| element == 'bar' } # => 3
```

Returns <tt>nil</tt> if the block never returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2]
a.rindex { |element| element == 'x' } # => nil
```

---

When neither an argument nor a block is given,
returns a new Enumerator:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
e = a.rindex
e # => #<Enumerator: [:foo, "bar", 2, "bar"]:rindex>
e.each { |element| element == 'bar' } # => 3
```

---

When both an argument and a block given,
gives a warning (<tt>warning: given block not used</tt>)
and ignores the block:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
index = a.rindex('bar') { fail 'Cannot happen' }
index # => 3
```
#### shift

```
ary.shift → obj or nil
ary.shift(n) → new_array
```

Removes and returns leading elements in <tt>ary</tt>.

The argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer.

---

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

---

If argument <tt>n</tt> is given and <tt>ary</tt> is empty,
returns a new empty Array:

```ruby
a = []
a.shift(1) # => []
a.shift(2) # => []
```

If argument <tt></tt> is given and <tt>ary</tt> is not empty,
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

If <tt>n</tt> is <tt>0</tt>,
returns an empty Array and <tt>ary</tt> is unmodified:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.shift(0)
a1 # => []
a # => [:foo, "bar", 2]
```

---

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

Returns elements from <tt>ary</tt>.

---

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

---

When the only arguments are Integers <tt>start</tt> and <tt>length</tt>,
returns a new array of size <tt>length</tt>
containing elements of <tt>ary</tt> beginning at index <tt>start</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice(0, 2) # => [:foo, "bar"]
a.slice(1, 2) # => ["bar", 2]
```

If <tt>start + length > ary.length</tt>,
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

---

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

---

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

May be chained:

```ruby
a = [:foo, 'bar', baz = 2]
a.unshift(:bam, :bat).unshift(:bad, :bah)
a # => [:bad, :bah, :bam, :bat, :foo, "bar", 2]
```
