## Array API

[Not yet complete. A few methods still to go.]

### Contents
- [Public Class Methods](#public-class-methods)
  - [::new](#new)
  - [::try_convert](#try_convert)
- [Public Instance Methods](#public-instance-methods)
  - [& (Intersection)](#-intersection)
  - [* (Repetition)](#-repetition)
  - [+ (Concatenation)](#-concatenation)
  - [- (Difference)](#--difference)
  - [<< (Append)](#-append)
  - [<=> (Comparison)](#-comparison)
  - [== (Equality)](#-equality)
  - [[] (Element Reference)](#-element-reference)
  - [[]= (Element Assignment)](#-element-assignment)
  - [append](#append)
  - [assoc](#assoc)
  - [at](#at)
  - [bsearch](#bsearch)
  - [bsearch_index](#bsearch_index)
  - [collect](#collect)
  - [collect!](#collect-1)
  - [combination](#combination)
  - [compact](#compact)
  - [compact!](#compact-1)
  - [concat](#concat)
  - [count](#count)
  - [cycle](#cycle)
  - [delete](#delete)
  - [delete_at](#delete_at)
  - [delete_if](#delete_if)
  - [difference](#difference)
  - [each](#each)
  - [each_index](#each_index)
  - [empty?](#empty)
  - [eql?](#eql)
  - [fetch](#fetch)
  - [fill](#fill)
  - [filter](#filter)
  - [filter!](#filter-1)
  - [find_index](#find_index)
  - [first](#first)
  - [flatten](#flatten)
  - [flatten!](#flatten-1)
  - [freeze](#freeze)
  - [hash](#hash)
  - [include?](#include)
  - [index](#index)
  - [insert](#insert)
  - [inspect](#inspect)
  - [intersection](#intersection)
  - [join](#join)
  - [keep_if](#keep_if)
  - [last](#last)
  - [length](#length)
  - [map](#map)
  - [map!](#map-1)
  - [max](#max)
  - [min](#min)
  - [minmax](#minmax)
  - [permutation](#permutation)
  - [pop](#pop)
  - [prepend](#prepend)
  - [push](#push)
  - [rassoc](#rassoc)
  - [reject](#reject)
  - [reject!](#reject-1)
  - [repeated_combination](#repeated_combination)
  - [repeated_permutation](#repeated_permutation)
  - [replace](#replace)
  - [reverse](#reverse)
  - [reverse!](#reverse-1)
  - [reverse_each](#reverse_each)
  - [rindex](#rindex)
  - [rotate](#rotate)
  - [rotate!](#rotate-1)
  - [select](#select)
  - [select!](#select-1)
  - [shift](#shift)
  - [slice](#slice)
  - [slice!](#slice-1)
  - [sort](#sort)
  - [sort!](#sort-1)
  - [sort_by!](#sort_by)
  - [to_a](#to_a)
  - [to_ary](#to_ary)
  - [to_h](#to_h)
  - [to_s](#to_s)
  - [transpose](#transpose)
  - [union](#union)
  - [uniq](#uniq)
  - [uniq!](#uniq-1)
  - [unshift](#unshift)
  - [values_at](#values_at)
  - [zip](#zip)
  - [| (Union)](#-union)

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
a = Array.new(3, :X)
a # => [:X, :X, :X]
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

#### & (Intersection)

```
ary & other_array→ new_array
```

Argument <tt>other_array</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects).

---
Returns a new Array containing each element found in both in <tt>ary</tt>
and in <tt>other_array</tt>; duplicates are omitted; comparisons use <tt>eql?</tt>:


```ruby
[0, 1, 2, 3] & [1, 2] # => [1, 2]
[0, 1, 0, 1] & [0, 1] # => [0, 1]
```

Preserves order from <tt>ary</tt>:

```ruby
[0, 1, 2] & [3, 2, 1, 0] # => [0, 1, 2]
```

---

Raises an exception if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[] & :foo # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### * (Repetition)

```
ary * an_integer → new_array
ary * a_string → new_string
```

The argument must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
which will be converted to an Integer, or a
[String-convertible object](../../../doc/convertibles.md#string-convertible-objects),
which will be converted to a String.

---

When the argument is an integer <tt>n</tt>,
returns a new Array.

If <tt>n</tt> is positive, returns the concatenation of <tt>n</tt>
repetitions of <tt>ary</tt>:

```ruby
a = ['x', 'y']
a1 = a * 3 # => ["x", "y", "x", "y", "x", "y"]
a1[0].object_id == a1[2].object_id # => true
```

If <tt>n</tt> is zero, returns an empty Array:

```ruby
a = [0, 1]
a * 0 # => []
```

---

When the argument is a string <tt>separator</tt>,
returns a new String equivalent to the result of
<tt>array.join(separator)</tt>.

If <tt>ary</tt> is non-empty, returns the join of each element's <tt>to_s</tt> value:

```ruby
[0, [0, 1], {foo: 0}] * ', ' # => "0, 0, 1, {:foo=>0}"
```

---

Raises an exception if the argument is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects)
or a
[String-convertible object](../../../doc/convertibles.md#string-convertible-objects):

```ruby
[] * :foo # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if the argument is negative:

```ruby
[] * -1 # Raises ArgumentError (negative argument)
```

Raises an exception if the argument is a string
and any array element lacks instance method <tt>to_s</tt>:

```ruby
[BasicObject.new] * ',' # Raises NoMethodError (undefined method `to_s' for #<BasicObject:>)
```

#### + (Concatenation)

```
ary + other_array → new_array
```

Argument <tt>other_array</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to an Array.

---

Returns a new Array containing all elements of <tt>ary</tt>
followed by all elements of <tt>other_array</tt>:

```ruby
a = [0, 1] + [2, 3]
a # => [0, 1, 2, 3]
```

---

Raises an exception if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[] + :foo # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### - (Difference)

```
ary - other_array → new_array
```

Argument <tt>other_array</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects).

---

Returns a new Array containing only those elements from <tt>ary</tt>
that are not <tt>eql?</tt> some element in <tt>other_array</tt>:

```ruby
[0, 1, 1, 2, 1, 1, 3, 1, 1] - [1] # => [0, 2, 3]
[0, 1, 2, 3] - [3, 0] # => [1, 2]
[0, 1, 2] - [4] # => [0, 1, 2]
```

---

Raises an exception if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[] - :foo # Raises TypeError (no implicit conversion of Symbol into Array)
```

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

#### <=> (Comparison)

```
ary <=> other_array → -1, 0, or 1
```

Returns <tt>-1</tt>, <tt>0</tt>, or <tt>1</tt>
as <tt>ary</tt> is less than, equal to, or greater than
<tt>other_array</tt>.

For each index <tt>n</tt> in <tt>ary</tt>,
evaluates <tt>result = ary[n] <=> other_ary[n]</tt>.

Returns <tt>-1</tt> for the first <tt>result</tt> of <tt>-1</tt>:

```ruby
[0, 1, 2] <=> [0, 1, 3] # => -1
```

Returns <tt>1</tt> for the first <tt>result</tt> of <tt>1</tt>:

```ruby
[0, 1, 2] <=> [0, 1, 1] # => 1
```

Returns <tt>-1</tt> if the <tt>result</tt> for each element in <tt>ary</tt>
is <tt>0</tt>, and <tt>ary.size < other_array.size</tt>:

```ruby
[0, 1, 2] <=> [0, 1, 2, 3] # => -1
```
Returns <tt>1</tt> if the <tt>result</tt> for each element in <tt>other_array</tt>
is <tt>0</tt>, and <tt>ary.size > other_array.size</tt>:

```ruby
[0, 1, 2] <=> [0, 1] # => 1
```

Returns <tt>0</tt> if each <tt>result</tt> is <tt>0</tt>,
and <tt>ary.size == other_array.size</tt>:

```ruby
[0, 1, 2] <=> [0, 1, 2] # => 0
```

Argument <tt>other_array</tt> may be an
[Array-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
require 'csv'
[] <=> CSV::Row.new([], []) # => 0
```

Returns <tt>nil</tt> if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[] <=> 0 # => nil
```

#### == (Equality)

```
ary == other_array → true or false
```

Returns <tt>true</tt> if <tt>ary.size == other_array.size</tt>,
and for each index <tt>i</tt> in <tt>ary</tt>, <tt>ary[i] == other_array[i]</tt>:

```ruby
a = [0, 1, 2]
a == [0, 1, 2] # => true
[] == [] # => true
```

Otherwise, returns <tt>false</tt>:

```ruby
a = [0, 1, 2]
a == [0, 1] # => false
a == [0, 1, 3] # => false
[] == BasicObject.new # => false
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
ary[index] = obj → obj
ary[start, length] = obj →  obj
ary[range] = obj →  obj
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

#### assoc

```
ary.assoc(obj) → new_array or nil
```

Returns the the first element in <tt>ary</tt> that is an Array
whose first element <tt>==</tt> <tt>>ary</tt>:

```ruby
a = [[0, 1], [2, 4], [4, 5, 6], [4, 5]]
a.assoc(4) # => [4, 5, 6]
```

Returns <tt>nil</tt> if no such element is found:

```ruby
a = [[0, 1], 7, {foo: 0}, [4, 5], [3, 4, 5]]
a.assoc(:nosuch) # => nil
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

#### bsearch

```
ary.bsearch { |element| ... } → obj
ary.bsearch → new_enumerator
```

Returns an element from <tt>ary</tt> selected by a binary search.

<tt>ary</tt> should be sorted, but this is not checked.

There are two search modes:
* Find-minimum mode:  the block should return <tt>true</tt> or <tt>false</tt>.
* Find-any mode:  the block should return a numeric value.

The block should not mix the modes by sometimes returning a numeric value
and sometimes returning <tt>true</tt> or <tt>false</tt>,
but this is not checked.

---

In find-minimum mode,
the block always returns <tt>true</tt> or <tt>false</tt>.
The further requirement (though not checked) is that
there are _no indexes_ <tt>i</tt> and <tt>j</tt> such that:
* <tt>0 <= i < j <= ary.size</tt>.
* The block returns <tt>true</tt> for <tt>ary[i]</tt> and <tt>false</tt> for <tt>ary[j]</tt>.

In find-minimum mode,
method <tt>bsearch</tt> returns the first element for which
the block returns <tt>true</tt>.

Examples:

```ruby
a = [0, 4, 7, 10, 12]
a.bsearch { |x| x >= 4 } # => 4
a.bsearch { |x| x >= 6 } # => 7
a.bsearch { |x| x >= -1 } # => 0
a.bsearch { |x| x >= 100 } # => nil
```

Less formally: the block is such that all <tt>false</tt>-evaluating elements
precede all <tt>true</tt>-evaluating elements.

These make sense as blocks for <tt>bsearch</tt>:

```ruby
a = [0, 4, 7, 10, 12]
a.map { |x| x >= 4 } # => [false, true, true, true, true]
a.map { |x| x >= 6 } # => [false, false, true, true, true]
a.map { |x| x >= -1 } # => [true, true, true, true, true]
a.map { |x| x >= 100 } # => [false, false, false, false, false]
```

This would not make sense:

```ruby
a = [0, 4, 7, 10, 12]
a.map {|x| x == 7 } # => [false, false, true, false, false]
```

---

In find-any mode, the block always returns a numeric value.
The further requirement (though not checked) is that
there are _no indexes_ <tt>i</tt> and <tt>j</tt> such that:
* <tt>0 <= i < j <= ary.size</tt>.
* The block returns a negative value for <tt>ary[i]</tt> and a positive value for <tt>ary[j]</tt>.
* The block returns a negative value for <tt>ary[i]</tt> and zero <tt>ary[j]</tt>.
* The block returns zero for <tt>ary[i]</tt> and a positive value for <tt>ary[j]</tt>.

In find-any mode,
method <tt>bsearch</tt> returns some element
for which the block returns <tt>0</tt>,
or <tt>nil></tt> if none.

Examples:

```ruby
a = [0, 4, 7, 10, 12]
a.bsearch { |element| 7 <=> element } # => 7
a.bsearch { |element| -1 <=> element } # => nil
a.bsearch { |element| 5 <=> element } # => nil
a.bsearch { |element| 15 <=> element } # => nil
```

Less formally: the block is such that:
* All positive-evaluating elements precede all zero-evaluating elements.
* All positive-evaluating elements precede all negative-evaluating elements.
* All zero-evaluating elements precede all negative-evaluating elements.

These make sense as blocks for <tt>bsearch</tt>:

```ruby
a = [0, 4, 7, 10, 12]
a.map { |element| 7 <=> element } # => [1, 1, 0, -1, -1]
a.map { |element| -1 <=> element } # => [-1, -1, -1, -1, -1]
a.map { |element| 5 <=> element } # => [1, 1, -1, -1, -1]
a.map { |element| 15 <=> element } # => [1, 1, 1, 1, 1]
```

This would not make sense:

```ruby
a = [0, 4, 7, 10, 12]
a.map { |element| element <=> 7 } # => [-1, -1, 0, 1, 1]
```

---

Returns an enumerator if no block given:

```
a = [0, 4, 7, 10, 12]
a.bsearch # => #<Enumerator: [0, 4, 7, 10, 12]:bsearch>
```
---

Raises an exception if the block returns an invalid value:

```ruby
a = 'abcde'.split('').shuffle
a.bsearch { |element| :foo } # Raises TypeError (wrong argument type Symbol (must be numeric, true, false or nil))
```

#### bsearch_index

```
ary.bsearch_index { |element| ... } → integer or nil
ary.bsearch → new_enumerator
```

Searches <tt>ary</tt> as described at method [bsearch](#bsearch),
but returns the index of the found element
instead of the element itself.

#### collect

```
ary.collect { |elemeent| ... } → new_array
ary.collect → new_enumerator
```

Returns a new Array whose elements are the return values
from the block:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.collect { |element| element.class }
a1 # => [Symbol, String, Integer]
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.collect
a1 # => #<Enumerator: [:foo, "bar", 2]:collect>
```

#### collect!

```
ary.collect! { |elemeent| ... } → self
ary.collect → new_enumerator
```

Replaces each element the return value from the block:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.collect! { |element| element.class }
a1 # => [Symbol, String, Integer]
a1.object_id == a.object_id # => true
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.collect!
a1 # => #<Enumerator: [:foo, "bar", 2]:collect!>
```

#### combination

```
ary.combination(n) { |element| ... } → self
ary.combination(n) → new_enumerator
```
Calls the block with combinations of elements of <tt>ary</tt>;
returns <tt>self</tt>.  The order of combinations is indeterminate.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When a block and a zero-valued argument <tt>n</tt> are given,
calls the block once with a new empty Array:

```ruby
a = [0, 1, 2]
a1 = a.combination(0) { |combination| p combination }
a1.equal?(a) # => true # Identity check.
```

Output:

```
[]
```

If <tt>0 < n < ary.size</tt>,
calls the block with all <tt>n</tt>-tuple combinations of <tt>ary</tt>:

```ruby
a = [0, 1, 2]
a.combination(2) { |combination| p combination }
```

Output:

```
[0, 1]
[0, 2]
[1, 2]
```

```ruby
a = [0, 1, 2]
a.combination(3) { |combination| p combination }
```

Output:

```
[0, 1, 2]
```

If <tt>n >= ary.size</tt>, does not call the block:

```ruby
a = [0, 1, 2]
a.combination(4) { |combination| fail 'Cannot happen' }
```

If <tt>n</tt> is negative, does not call the block:

```ruby
a = [0, 1, 2]
a.combination(-1) { |combination| fail 'Cannot happen' }
a.combination(-2) { |combination| fail 'Cannot happen' }
```

---

Returns a new Enumerator if no block given:

```ruby
a = [0, 1, 2]
a.combination(2) # => #<Enumerator: [0, 1, 2]:combination(2)>
```

---

Raises an exception if <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a.combination(:foo) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### compact

```
ary.compact → self or nil
```
Returns a new Array containing all non-<tt>nil</tt>
from <tt>ary</tt>:

```ruby
a = [nil, 0, nil, 1, nil, 2, nil]
a.compact # => [0, 1, 2]
```

#### compact!

```
ary.compact! → self or nil
```

Removes all <tt>nil</tt> elements from <tt>ary</tt>.

Returns <tt>self</tt> if any elements removed:

```ruby
a = [nil, 0, nil, 1, nil, 2, nil]
a.compact! # => [0, 1, 2]
```

Returns <tt>nil</tt> if no elements removed:

```ruby
[0, 1, 2].compact! # => nil
```

#### concat

```
ary.concat(*other_arrays) → self
```

The given <tt>*other_arrays</tt> must be
[Array-convertible objects](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to Array objects.

---

Adds to <tt>ary</tt> all elements from each array in <tt>other_arrays</tt>;
returns <tt>self</tt>:

```ruby
a = [0, 1]
a1 = a.concat([2, 3], [4, 5])
a1 # => [0, 1, 2, 3, 4, 5]
a1.object_id == a.object_id # => true 
```

Returns <tt>self</tt> unmodified if no arguments given:

```ruby
a = [0, 1]
a.concat
a # => [0, 1]
```

---

Raises an exception if any argument is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[].concat([], :foo) # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### count

```
ary.count → an_integer
ary.count(obj) → an_integer
ary.count { |element| ... } → an_integer
```

Returns a count of specified elements.

With no argument and no block,
returns the count of all elements:

```ruby
[0, 1, 2].count # => 3
[].count # => 0
```

With argument <tt>obj</tt>,
returns the count of elements <tt>eql?</tt> to <tt>obj</tt>:

```ruby
[0, 1, 2, 0].count(0) # => 2
[0, 1, 2].count(3) # => 0
```

#### cycle

```
ary.cycle { |element| ... } → nil
ary.cycle(count) { |element| ... } → nil
ary.cycle → new_enumerator
ary.cycle(count) → new_enumerator
```

Argument <tt>count</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When called with positive argument <tt>count</tt> and a block,
calls the block with each element, then does so again,
until it has done so <tt>count</tt> times returns <tt>nil</tt>:

```ruby
[0, 1].cycle(2) { |element| puts element } # => nil
```

Output:

```
0
1
0
1
```

If <tt>count</tt> is zero or negative, does not call the block:

```ruby
[0, 1].cycle(0) { |element| fail 'Cannot happen' } # => nil
[0, 1].cycle(-1) { |element| fail 'Cannot happen' } # => nil
```

---

When a block is given, but no argument, cycles forever
(output omitted):

```
[0, 1].cycle { |element| puts element }
```

---

When a block is not given, returns a new Enumerator:

```ruby
[0, 1].cycle(2) # => #<Enumerator: [0, 1]:cycle(2)>
[0, 1].cycle # => #<Enumerator: [0, 1]:cycle(2)> # => #<Enumerator: [0, 1]:cycle>
```

---

Raises an exception if <tt>count</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1].cycle(:foo) { |element| } # Raises TypeError (no implicit conversion of Symbol into Integer)
```


#### delete

```
ary.delete(obj) → deleted_obj
ary.delete { |obj| ... } → deleted_obj or block_return
```

Removes zero or more elements from <tt>ary</tt>; returns <tt>self</tt>.

---

When no block given,
removes from <tt>ary</tt> each element <tt>ele</tt>
such that <tt>ele == obj</tt>; returns the last deleted element:

```ruby
s1 = 'bar'; s2 = 'bar'
a = [:foo, s1, baz = 2, s2]
deleted_obj = a.delete('bar')
a # => [:foo, 2]
deleted_obj.object_id == s2.object_id # => true
```

Returns <tt>nil</tt> if nothing removed:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete(:nosuch) # => nil
```

---

When block given,
removes from <tt>ary</tt> each element <tt>ele</tt>
such that <tt>ele == obj</tt>; returns the last deleted element:

```ruby
s1 = 'bar'; s2 = 'bar'
a = [:foo, s1, baz = 2, s2]
deleted_obj = a.delete('bar') { fail 'Cannot happen' }
a # => [:foo, 2]
deleted_obj.object_id == s2.object_id # => true
```

Returns block return value if nothing removed:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete(:nosuch) { |obj| "#{obj} not found" } # => "nosuch not found"
```

#### delete_at

```
ary.delete_at(index) → deleted_object or nil
```

Deletes an element from <tt>ary</tt>, per the given <tt>index</tt>.

The given <tt>index</tt> must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When <tt>index</tt> is non-negative,
deletes the element at offset <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete_at(1) # => "bar"
a # => [:foo, 2]
```

If index is too large, returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete_at(5) # => nil
```

---

When <tt>index</tt> is negative,
counts backward from the end of the array:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete_at(-2) # => "bar"
a # => [:foo, 2]
```

If index is too small, returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete_at(-5) # => nil
```

---

Raises an exception if <tt>index</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a.delete_at(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### delete_if

```
ary.delete_if { |element| ... } → self
ary.delete_if → Enumerator
```

Removes each element in <tt>ary</tt> for which the block
returns a truthy value; returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2, 'bat']
a1 = a.delete_if { |element| element.to_s.start_with?('b') }
a1 # => [:foo, 2]
a1.object_id == a.object_id # => true
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a.delete_if # => #<Enumerator: [:foo, "bar", 2]:delete_if>
```

#### difference

```
ary.difference(*other_arrays) → new_array
```

Each argument in <tt>*other_arrays</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects).

---

Returns a new Array containing only those elements from <tt>ary</tt>
that are not <tt>eql?</tt> some element in any <tt>*other_arrays</tt>:

```ruby
[0, 1, 1, 2, 1, 1, 3, 1, 1].difference([1]) # => [0, 2, 3]
[0, 1, 2, 3].difference([3, 0], [1, 3]) # => [2]
[0, 1, 2].difference([4]) # => [0, 1, 2]
```

Returns <tt>self</tt> unmodified if no arguments given:

```ruby
[0, 1].difference # => [0, 1]
```

---

Raises an exception if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[].difference(:foo) # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### each

```
ary.each { |element| ... } → self
ary.each → Enumerator
```

When a block given,
passes each successive <tt>element</tt> in <tt>ary</tt>
to the block; returns <tt>self</tt>:

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
a.each { |element| puts element; a.clear if element.to_s.start_with?('b') }
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
ary.each_index { |index| ... } → self
ary.each_index → Enumerator
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

#### empty?

```
ary.empty? → true or false
```

Returns <tt>true</tt> if the count of elements in <tt>ary</tt> is <tt>0</tt>;
<tt>false</tt> otherwise:

```ruby
[].empty? # => true
[:foo, 'bar', baz = 2].empty? # => false
```

#### eql?

```
ary.eql other_array → true or false
```

Returns <tt>true</tt> if <tt>ary.size == other_array.size</tt>,
and for each index <tt>i</tt> in <tt>ary</tt>, <tt>ary[i].eql? other_array[i]</tt>:

```ruby
a = [0, 1, 2]
a.eql? [0, 1, 2] # => true
[].eql? [] # => true
```

Otherwise, returns <tt>false</tt>:

```ruby
a = [0, 1, 2]
a.eql? [0, 1] # => false
a.eql? [0, 1, 3] # => false
a.eql? [0, 1, 3] # => false
[].eql? BasicObject.new # => false
```

#### fetch

```
ary.fetch(index) → obj
ary.fetch(index, default_value) → obj
ary.fetch(index) { |index| ... } → obj
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
a.fetch(:X) # Raises TypeError (no implicit conversion of String into Integer)
```

Raises an exception if <tt>index</tt> is out of range
and neither <tt>default_value</tt> nor a block given:

```ruby
a = [:foo, 'bar', baz = 2]
a.fetch(50) # Raises IndexError (index 50 outside of array bounds: -3...3)
```

#### fill

```
ary.fill(obj) → self
ary.fill(obj, start) → self
ary.fill(obj, start , length) → self
ary.fill(obj, range) → self
ary.fill { |index| ... } → self
ary.fill(start) { |index| ... } → self
ary.fill(start , length) { |index| ... } → self
ary.fill(range) { |index| ... } → self
```

Replaces specified elements in <tt>ary</tt> with specified objects;
returns <tt>self</tt>.

Arguments <tt>start</tt> and <tt>length</tt>, if given, must be
[Integer-convertible objects](../../../doc/convertibles.md#integer-convertible-objects).

Argument <tt>range</tt>, if given, must be a Range object.

---

With argument <tt>obj</tt> and no block given,
replaces all elements with that one <tt>obj</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a # => ["a", "b", "c", "d"]
a1 = a.fill(:X)
a1 # => [:X, :X, :X, :X]
a.first.object_id == a.last.object_id # => true
```

---

With arguments <tt>obj</tt> and <tt>start</tt> and no block given,
replaces elements based on the given <tt>start</tt>.

If <tt>start</tt> is in range
(<tt>0 <= start < ary.size</tt>),
replaces all elements from offset <tt>start</tt> through the end:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, 2) # => ["a", "b", :X, :X]
```

If <tt>start >= ary.size</tt>, does nothing:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, 4) # => ["a", "b", "c", "d"]
a = ['a', 'b', 'c', 'd']
a.fill(:X, 5) # => ["a", "b", "c", "d"]
```

If <tt>start</tt> is negative, counts from the end
(starting index is <tt>start + ary.size</tt>):

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, -2) # => ["a", "b", :X, :X]
```

If <tt>start <= -ary.size</tt>, replaces all elements:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, -6) # => [:X, :X, :X, :X]
a = ['a', 'b', 'c', 'd']
a.fill(:X, -50) # => [:X, :X, :X, :X]
```

---

With arguments <tt>obj</tt>, <tt>start</tt>, and <tt>length</tt>,
and no block given,
replaces elements based on the given <tt>start</tt> and <tt>length</tt>.

If <tt>start</tt> is in range,
replaces <tt>length</tt> elements beginning at offset <tt>start</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, 1, 1) # => ["a", :X, "c", "d"]
```

If <tt>start</tt> is negative, counts from the end:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, -2, 1) # => ["a", "b", :X, "d"]
```

If <tt>start >= ary.size</tt>, extends <tt>self</tt> with <tt>nil</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, 5, 0) # => ["a", "b", "c", "d", nil]
a = ['a', 'b', 'c', 'd']
a.fill(:X, 5, 2) # => ["a", "b", "c", "d", nil, :X, :X]
```

If <tt>length <= 0</tt> replaces no elements:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, 1, 0) # => ["a", "b", "c", "d"]
a.fill(:X, 1, -1) # => ["a", "b", "c", "d"]
```

---

With arguments <tt>obj</tt> and <tt>range</tt> and no block given
replaces elements based on the given <tt>range</tt>.

If the range is positive and ascending
(<tt> 0 <= range.begin <= range.end</tt>,
replaces elements from (tt>range.begin</tt> to <tt>range.end</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, (1..1)) # => ["a", :X, "c", "d"]
```

If <tt>range.first</tt> is negative,
replaces no elements:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, (-1..1)) # => ["a", "b", "c", "d"]
```

If <tt>range.last</tt> is negative,
counts from the end of the array:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, (0..-2)) # => [:X, :X, :X, "d"]
a = ['a', 'b', 'c', 'd']
a.fill(:X, (1..-2)) # => ["a", :X, :X, "d"]
```

If <tt>range.last</tt> and <tt>range.last</tt> are both negative,
both count from the end of the array:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(:X, (-1..-1)) # => ["a", "b", "c", :X]
a = ['a', 'b', 'c', 'd']
a.fill(:X, (-2..-2)) # => ["a", "b", :X, "d"]
```

---

With no argument and block given,
calls the block with each <tt>index</tt>
and replaces <tt>self[index]</tt> with the block's return value:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill { |index| "new_#{index}" } # => ["new_0", "new_1", "new_2", "new_3"]
```

---

With argument <tt>start</tt> and block given,
calls the block with each <tt>index</tt> from <tt>start</tt> to the end,
replacing the corresponding element with the block's return value:

If <tt>start</tt> is in range
(<tt>0 <= start < ary.size</tt>),

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(1) { |index| "new_#{index}" } # => ["a", "new_1", "new_2", "new_3"]
```

If <tt>start >= ary.size</tt>, does nothing:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(4) { |index| fail 'Cannot happen' } # => ["a", "b", "c", "d"]
a = ['a', 'b', 'c', 'd']
a.fill(4) { |index| fail 'Cannot happen' } # => ["a", "b", "c", "d"]
```

If <tt>start</tt> is negative, counts from the end
(starting index is <tt>start + ary.size</tt>):

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(-2) { |index| "new_#{index}" } # => ["a", "b", "new_2", "new_3"]
```

If <tt>start <= -ary.size</tt>, replaces all elements:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(-6) { |index| "new_#{index}" } # => ["new_0", "new_1", "new_2", "new_3"]
a = ['a', 'b', 'c', 'd']
a.fill(-50) { |index| "new_#{index}" } # => ["new_0", "new_1", "new_2", "new_3"]
```

---

With arguments <tt>start</tt> and <tt>length</tt>,
and block given,
calls the block for each <tt>index</tt> specified by <tt>start</tt> <tt>length</tt>;
replaces the element at offset <tt>index</tt> with the block's return value.

If <tt>start</tt> is in range,
replaces <tt>length</tt> elements beginning at offset <tt>start</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(1, 1) { |index| "new_#{index}" } # => ["a", "new_1", "c", "d"]
```

If <tt>start</tt> is negative, counts from the end:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(-2, 1) { |index| "new_#{index}" } # => ["a", "b", "new_2", "d"]
```

If <tt>start >= ary.size</tt>, extends <tt>self</tt> with <tt>nil</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(5, 0) { |index| "new_#{index}" } # => ["a", "b", "c", "d", nil]
a = ['a', 'b', 'c', 'd']
a.fill(5, 2) { |index| "new_#{index}" } # => ["a", "b", "c", "d", nil, "new_5", "new_6"]
```

If <tt>length <= 0</tt> replaces no elements:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(1, 0) { |index| "new_#{index}" } # => ["a", "b", "c", "d"]
a.fill(1, -1) { |index| "new_#{index}" } # => ["a", "b", "c", "d"]
```

---

With arguments <tt>obj</tt> and <tt>range</tt> and block given
calls the block with each <tt>index</tt> in the given <tt>range</tt>,
replaces the element at offset <tt>index</tt> with the block's return value.

If the range is positive and ascending
(<tt> 0 <= range.begin <= range.end</tt>,
replaces elements from (tt>range.begin</tt> to <tt>range.end</tt>:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(1..1) { |index| "new_#{index}" } # => ["a", "new_1", "c", "d"]
```

If <tt>range.first</tt> is negative, does not call the block,
replaces no elements:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(-1..1) { |index| fail 'Cannot happen' } # => ["a", "b", "c", "d"]
```

If <tt>range.last</tt> is negative,
counts from the end of the array:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(0..-2) { |index| "new_#{index}" } # => ["new_0", "new_1", "new_2", "d"] 
a = ['a', 'b', 'c', 'd']
a.fill(1..-2) { |index| "new_#{index}" } # => ["a", "new_1", "new_2", "d"]
```

If <tt>range.last</tt> and <tt>range.last</tt> are both negative,
both count from the end of the array:

```ruby
a = ['a', 'b', 'c', 'd']
a.fill(-1..-1) { |index| "new_#{index}" } # => ["a", "b", "c", "new_3"]
a = ['a', 'b', 'c', 'd']
a.fill(-2..-2) { |index| "new_#{index}" } # => ["a", "b", "new_2", "d"]
```

---

Raises an exception if no block given, two arguments given,
second argument is not a Range or an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[].fill(:X, :x) # Raises TypeError (no implicit conversion of Symbol into Integer)
```
Raises an exception if no block given, three arguments given,
second or third argument not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[].fill(:X, :x, 1) # Raises TypeError (no implicit conversion of Symbol into Integer)
[].fill(:X, 1, :x) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if block given, one argument given,
argument is not a Range or an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[].fill(:x) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if block given, two arguments given,
either argument is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[].fill(:x, 1) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
[].fill(1, :x) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
```


#### filter

```
ary.filter { |element| ... } → new_array
ary.filter → new_enumeration
```

Returns a new Array containing those elements of <tt>ary</tt>
for which the block returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a1 = a.filter { |element| element.to_s.start_with?('b') }
a1 # => ["bar", :bam]
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a.filter # => #<Enumerator: [:foo, "bar", 2, :bam]:filter>
```

#### filter!

```
ary.filter! { |element| ... } → new_array
ary.filter! → new_enumeration
```

Removes from <tt>ary</tt> those elements for which the block
returns <tt>false</tt> or <tt>nil</tt>; returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a1 = a.filter! { |element| element.to_s.start_with?('b') }
a1 # => ["bar", :bam]
a1.object_id == a.object_id 
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a.filter! # => #<Enumerator: [:foo, "bar", 2, :bam]:filter!>
```

#### find_index

```
ary.find_index(obj) → Integer or nil
ary.find_index { |element| ... } → Integer or nil
ary.find_index → new_enumerator
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
a.find_index { |element| element == :X } # => nil
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
a.first(:X) # Raises TypeError (no implicit conversion of String into Integer)
```

#### flatten

```
ary.flatten → self or nil
ary.flatten(level) → self or nil
```

Replaces each Array object in <tt>ary</tt> with
the elements from that object; returns <tt>self</tt>
if any changes, <tt>nil</tt> otherwise.

Argument <tt>level</tt>, if given must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

With no argument, flattens all levels:

```ruby
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a1 = a.flatten
a1 # => [0, 1, 2, 3, 4, 5]
a1.equal?(a) # => true # Identity check
[0, 1, 2].flatten # => nil
```

With non-negative argument <tt>level</tt>,
flattens recursively through <tt>level</tt> levels:

```ruby
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten(0) # => nil
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten(1) # => [0, 1, [2, 3], 4, 5]
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten(2) # => [0, 1, 2, 3, 4, 5]
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten(3) # => [0, 1, 2, 3, 4, 5]
[0, 1, 2].flatten(1) # => nil
```

With negative argument <tt>level</tt>,
flattens all levels:

```ruby
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten(-1) # => [0, 1, 2, 3, 4, 5]
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten(-2) # => [0, 1, 2, 3, 4, 5]
[0, 1, 2].flatten(-1) # => nil
```

---

Raises an exception if <tt>level</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[].flatten(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if <tt>ary</tt> contains a circular reference:

```ruby
a = []
a.push([a, a])
a.flatten # Raises ArgumentError (tried to flatten recursive array)
```

Raises an exception if <tt>flatten</tt> reentered:

```ruby
require 'continuation'
o = Object.new
def o.to_ary() callcc {|k| @cont = k; [1,2,3]} end
[10, 20, o, 30, o, 40].flatten
o.instance_eval {@cont}.call # Raises RuntimeError (flatten reentered)
```

#### flatten!

```
ary.flatten! → self or nil
ary.flatten!(level) → self or nil
```

Replaces each Array object in <tt>ary</tt> with
the elements from that object; returns <tt>self</tt>
if any changes, <tt>nil</tt> otherwise.

Argument <tt>level</tt>, if given must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

With no argument, flattens all levels:

```ruby
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a1 = a.flatten!
a1 # => [0, 1, 2, 3, 4, 5]
a1.equal?(a) # => true # Identity check
[0, 1, 2].flatten! # => nil
```

With non-negative argument <tt>level</tt>,
flattens recursively through <tt>level</tt> levels:

```ruby
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten!(0) # => nil
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten!(1) # => [0, 1, [2, 3], 4, 5]
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten!(2) # => [0, 1, 2, 3, 4, 5]
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten!(3) # => [0, 1, 2, 3, 4, 5]
[0, 1, 2].flatten!(1) # => nil
```

With negative argument <tt>level</tt>,
flattens all levels:

```ruby
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten!(-1) # => [0, 1, 2, 3, 4, 5]
a = [ 0, [ 1, [2, 3], 4 ], 5 ]
a.flatten!(-2) # => [0, 1, 2, 3, 4, 5]
[0, 1, 2].flatten!(-1) # => nil
```

---

Raises an exception if <tt>level</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[].flatten!(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer) 
```

Raises an exception if <tt>ary</tt> contains a circular reference:

```ruby
a = []
a.push([a, a])
a.flatten! # Raises ArgumentError (tried to flatten recursive array)
```

Raises an exception if <tt>flatten</tt> reentered:

```ruby
require 'continuation'
o = Object.new
def o.to_ary() callcc {|k| @cont = k; [1,2,3]} end
[10, 20, o, 30, o, 40].flatten!
o.instance_eval {@cont}.call # Raises RuntimeError (flatten reentered)
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

---

Raises an exception for an attempt to modify a frozen Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.freeze
a[3] = :bat # Raises FrozenError (can't modify frozen Array: [:foo, "bar", 2])
```

#### hash

```
ary.hash → integer
```

Returns the integer hash value for <tt>ary</tt>:

```ruby
[].hash.class # => Integer
```

Two arrays have the same hash value if and only if they have the same content:

```ruby
[0, 1, 2].hash == [0, 1, 2].hash # => true
[0, 1, 2].hash == [0, 1, ].hash # => false
```

#### include?

```
ary.include?(obj) → true or false
```

Returns <tt>true</tt> if <tt>obj == ary[n]</tt> for some index <tt>n</tt>:

```ruby
[0, 1, 2].include?(2) # => true
[0, 1, 2].include?(3) # => false
```

#### index

```
ary.index(obj) → Integer or nil
ary.index { |element| ... } → Integer or nil
ary.index → new_enumerator
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
a.index { |element| element == :X } # => nil
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
ary.insert(index, *objects) → self
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

#### inspect

```
ary.inspect → new_string
```

Returns a new String formed by calling method <tt>inspect</tt>
on each element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.inspect  # => "[:foo, \"bar\", 2]"
```

Raises an exception if any element lacks instance method <tt>inspect</tt>:

```ruby
a = [:foo, 'bar', baz = 2, BasicObject.new]
a.inspect  # Raises NoMethodError (undefined method `inspect' for #<BasicObject:0x0000000006b69d28>)
```

#### intersection

```
ary.intersection(*other_arrays) → new_array
```

Each object in <tt>*other_arrays</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to an Array.

---

Returns a new Array containing each element found in both in <tt>ary</tt>
and in all of the given <tt>*other_arrays</tt>; duplicates are omitted; comparisons use <tt>eql?</tt>:

```ruby
[0, 1, 2, 3].intersection([0, 1, 2], [0, 1, 3]) # => [0, 1]
[0, 0, 1, 1, 2, 3].intersection([0, 1, 2], [0, 1, 3]) # => [0, 1]
```

Preserves order from <tt>ary</tt>:

```ruby
[0, 1, 2].intersection([2, 1, 0]) # => [0, 1, 2]
```

---

Raises an exception if any of the given <tt>*other_arrays</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[].intersection([0, 1], :foo) # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### join

```
ary.join → new_string
ary.join(separator) → new_string
```

Returns a new String formed by joining the elements of <tt>ary</tt>.

Argument <tt>separator</tt>, if given, must be a
[String-convertible object](../../../doc/convertibles.md#string-convertible-objects):


---

With no argument, joins using the output field separator, <tt>$,</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
$, # => nil
a.join # => "foobar2"
```

---

With argument <tt>separator</tt>, joins using that separator:

```ruby
a = [:foo, 'bar', baz = 2]
a.join("\n") # => "foo\nbar\n2"
```

---

Raises an exception if <tt>separator</tt> is not a
[String-convertible object](../../../doc/convertibles.md#string-convertible-objects):


```ruby
a = [:foo, 'bar', baz = 2]
a.join(:foo) # Raises TypeError (no implicit conversion of Symbol into String)
```

Raises an exception if any element lacks instance method <tt>to_s</tt>:

```ruby
a = [:foo, 'bar', baz = 2, BasicObject.new]
a.join # Raises NoMethodError (undefined method `to_s' for #<BasicObject:>)
```

#### keep_if

```
ary.keep_if { |element| ... } → new_array
ary.keep_if → new_enumeration
```

Removes from <tt>ary</tt> those elements for which the block
returns <tt>false</tt> or <tt>nil</tt>; returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a1 = a.keep_if { |element| element.to_s.start_with?('b') }
a1 # => ["bar", :bam]
a1.object_id == a.object_id 
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a.keep_if # => #<Enumerator: [:foo, "bar", 2, :bam]:keep_if>
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
a.last(:X) # Raises TypeError (no implicit conversion of String into Integer)
```

#### length

```
ary.length → int
```

Returns the count of elements in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.length # => 3
[].length # => 0
```

#### map

```
ary.map { |elemeent| ... } → new_array
ary.map → new_enumerator
```

Returns a new Array whose elements are the return values
from the block:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.map { |element| element.class }
a1 # => [Symbol, String, Integer]
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.map
a1 # => #<Enumerator: [:foo, "bar", 2]:map>
```

#### map!

```
ary.map! { |elemeent| ... } → self
ary.map → new_enumerator
```

Replaces each element the return value from the block:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.map! { |element| element.class }
a1 # => [Symbol, String, Integer]
a1.object_id == a.object_id # => true
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.map!
a1 # => #<Enumerator: [:foo, "bar", 2]:map!>
```


#### max

```
ary.max → obj
ary.max { |a, b| ... } → obj
ary.max(n) → new_array
ary.max(n) { |a, b| ... } → new_array
```

If no block given, each element in <tt>ary</tt> must respond to method <tt><=></tt> with an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
and may not be negative.

The block, if given, must return an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

With no argument and no block, returns the element in <tt>ary</tt>
having the maximum value per <tt><=></tt>:

```ruby
[0, 1, 2].max # => 2
```

With an argument <tt>n</tt> and no block,
returns a new Array with at most <tt>n</tt> elements:

```ruby
[0, 1, 2, 3].max(3) # => [3, 2, 1]
[0, 1, 2, 3].max(6) # => [3, 2, 1]
[0, 1, 2, 3].max(0) # => []
```

With a block and no argument,
calls the block <tt>ary.size-1</tt> times
to compare elements;
returns the element having the maximum value per the block.

```ruby
['0', '00', '000'].max { |a, b| a.size <=> b.size } # => "000" 
```

With an argument <tt>n</tt> and a block,
returns a new Array with at most <tt>n</tt> elements:

```ruby
['0', '00', '000'].max(2) { |a, b| a.size <=> b.size } # => ["000", "00"]
['0', '00', '000'].max(0) { |a, b| a.size <=> b.size } # => []
```

---

Raises an exception on encountering elements that are not comparable:

```ruby
[0, 1, :foo].max # Raises ArgumentError (comparison of Symbol with 1 failed)
```

Raises an exception if argument <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1].max(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if argument <tt>n</tt> is negative:

```ruby
[0, 1].max(-1) # Raises ArgumentError (negative size (-1))
```

Raises an exception if the block returns an object that is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1, 2].max { |a, b| :foo } # Raises ArgumentError (comparison of Symbol with 0 failed)
```

#### min

```
ary.min → obj
ary.min { |a, b| ... } → obj
ary.min(n) → new_array
ary.min(n) { |a, b| ... } → new_array
```

If no block given, each element in <tt>ary</tt> must respond to method <tt><=></tt> with an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects),
and may not be negative.

The block, if given, must return an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

With no argument and no block, returns the element in <tt>ary</tt>
having the minimum value per <tt><=></tt>:

```ruby
[0, 1, 2].min # => 0
```

---

With an argument <tt>n</tt> and no block,
returns a new Array with at most <tt>n</tt> elements:

```ruby
[0, 1, 2, 3].min(3) # => [0, 1, 2]
[0, 1, 2, 3].min(6) # => [0, 1, 2, 3]
[0, 1, 2, 3].min(0) # => []
```

---

With a block and no argument,
calls the block <tt>ary.size-1</tt> times
to compare elements;
returns the element having the minimum value per the block.

```ruby
['0', '00', '000'].min { |a, b| a.size <=> b.size } # => "0"
```

---

With an argument <tt>n</tt> and a block,
returns a new Array with at most <tt>n</tt> elements:

```ruby
['0', '00', '000'].min(2) { |a, b| a.size <=> b.size } # => ["0", "00"]
['0', '00', '000'].min(0) { |a, b| a.size <=> b.size } # => []
```

---
Raises an exception on encountering elements that are not comparable:

```ruby
[0, 1, :foo].min # Raises ArgumentError (comparison of Symbol with 1 failed)
```

Raises an exception if argument <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1].min(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if argument <tt>n</tt> is negative:

```ruby
[0, 1].min(-1) # Raises ArgumentError (negative size (-1))
```

Raises an exception if the block returns an object that is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1, 2].max { |a, b| :foo } # Raises ArgumentError (comparison of Symbol with 0 failed)
```

#### minmax

```
ary.minmax → [min_val, max_val]
ary.min { |a, b| ... } → [min_val, max_val]
```

If no block given, each element in <tt>ary</tt> must respond to method <tt><=></tt> with an
Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

The block, if given, must return an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

With no block, returns the elements in <tt>ary</tt>
having the minimum and maximum values per <tt><=></tt>:

```ruby
[0, 1, 2].min_max # => [0, 2]
```

With a block, calls the block <tt>ary.size-1</tt> times
to compare elements;
returns the elements in <tt>ary</tt>
having the minimum and maximum values per <tt><=></tt>:

```ruby
['0', '00', '000'].min_max { |a, b| a.size <=> b.size } # => ["0", "000"]
```

---
Raises an exception on encountering elements that are not comparable:

```ruby
[0, 1, :foo].min # Raises ArgumentError (comparison of Symbol with 1 failed)
```

Raises an exception if argument <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1].min(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if argument <tt>n</tt> is negative:

```ruby
[0, 1].min(-1) # Raises ArgumentError (negative size (-1))
```

Raises an exception if the block returns an object that is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
[0, 1, 2].max { |a, b| :foo } # Raises ArgumentError (comparison of Symbol with 0 failed)
```

#### permutation

```
ary.permutation { |element| ... } → self
ary.permutation(n) { |element| ... } → self
ary.permutation → new_enumerator
ary.permutation(n) → new_enumerator
```
Calls the block with permutations of elements of <tt>ary</tt>;
returns <tt>self</tt>.  The order of permutations is indeterminate.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When a block and a zero-valued argument <tt>n</tt> are given,
calls the block once with a new empty Array:

```ruby
a = [0, 1, 2]
a1 = a.permutation(0) { |permutation| p permutation }
a1.equal?(a) # => true # Identity check.
```

Output:

```
[]
```

If <tt>0 < n < ary.size</tt>,
calls the block with all <tt>n</tt>-tuple permutations of <tt>ary</tt>:

```ruby
a = [0, 1, 2]
a.permutation(2) { |permutation| p permutation }
```

Output:

```
[0, 1]
[0, 2]
[1, 0]
[1, 2]
[2, 0]
[2, 1]
```

```ruby
a = [0, 1, 2]
a.permutation(3) { |permutation| p permutation }
```

Output:

```
[0, 1, 2]
[0, 2, 1]
[1, 0, 2]
[1, 2, 0]
[2, 0, 1]
[2, 1, 0]
```

If <tt>n >= ary.size</tt>, does not call the block:

```ruby
a = [0, 1, 2]
a.permutation(4) { |permutation| fail 'Cannot happen' }
```

If <tt>n</tt> is negative, does not call the block:

```ruby
a = [0, 1, 2]
a.permutation(-1) { |permutation| fail 'Cannot happen' }
a.permutation(-2) { |permutation| fail 'Cannot happen' }
```

When a block given but no argument,
behaves the same as <tt>a.permutation(a.size)</tt>:

```ruby
a = [0, 1, 2]
a.permutation { |permutation| p permutation }
```

Output:

```
[0, 1, 2]
[0, 2, 1]
[1, 0, 2]
[1, 2, 0]
[2, 0, 1]
[2, 1, 0]
```

---

Returns a new Enumerator if no block given:

```ruby
a = [0, 1, 2]
a.permutation(2) # => #<Enumerator: [0, 1, 2]:permutation(2)>
```

---

Raises an exception if <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a.permutation(:foo) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
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

#### rassoc

```
ary.assoc(obj) → new_array or nil
```

Returns the the first element in <tt>ary</tt> that is an Array
whose second element <tt>==</tt> <tt>>ary</tt>:

```ruby
a = [[0, 1], [2, 4], [4, 5]]
a.rassoc(4) # => [2, 4]
```

Returns <tt>nil</tt> if no such element is found:

```ruby
a = [[0, 1], 7, {foo: 0}, [4, 5]]
a.rassoc(:nosuch) # => nil
```

#### reject

```
ary.reject { |element| ... } → new_array
ary.reject → new_enumerator
```

Returns a new Array whose elements are all those from <tt>ary</tt>
for which the block returns <tt>false</tt> or <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2, 'bat']
a1 = a.reject { |element| element.to_s.start_with?('b') }
a1 # => [:foo, 2]
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a.reject # => #<Enumerator: [:foo, "bar", 2]:reject>
```

#### reject!

```
ary.reject! { |element| ... } → self or nil
ary.reject! → new_enumerator
```

Removes each element for which the block returns a truthy value.

Returns <tt>self</tt> if any elements removed:

```ruby
a = [:foo, 'bar', baz = 2, 'bat']
a1 = a.reject! { |element| element.to_s.start_with?('b') }
a1 # => [:foo, 2]
a1.object_id == a.object_id # => true
```

Returns <tt>nil</tt> if no elements removed:

```ruby
a = [:foo, 'bar', baz = 2]
a.reject! { |element| false } # => nil
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2]
a.reject! # => #<Enumerator: [:foo, "bar", 2]:reject!>
```

#### repeated_combination

```
ary.repeated_combination(n) { |element| ... } → self
ary.repeated_combination(n) → new_enumerator
```
Calls the block with repeated combinations of length <tt>n</tt>
of the elements of <tt>ary</tt>;
returns <tt>self</tt>.  The order of repeated combinations is indeterminate.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When a block and a zero-valued argument <tt>n</tt> are given,
calls the block once with a new empty Array:

```ruby
a = [0, 1, 2]
a1 = a.repeated_combination(0) { |combination| p combination }
a1.equal?(a) # => true # Identity check.
```

Output:

```
[]
```

If <tt>0 < n < ary.size</tt>,
calls the block with all <tt>n</tt>-tuple repeated combinations of <tt>ary</tt>:

```ruby
a = [0, 1, 2]
a.repeated_combination(2) { |combination| p combination }
```

Output:

```
[0, 0]
[0, 1]
[0, 2]
[1, 1]
[1, 2]
[2, 2]
```

```ruby

a = [0, 1, 2]
a.repeated_combination(3) { |combination| p combination }
```

Output:

```
[0, 0, 0]
[0, 0, 1]
[0, 0, 2]
[0, 1, 1]
[0, 1, 2]
[0, 2, 2]
[1, 1, 1]
[1, 1, 2]
[1, 2, 2]
[2, 2, 2]```

If <tt>n</tt> is negative, does not call the block:

```ruby
a = [0, 1, 2]
a.repeated_combination(-1) { |combination| fail 'Cannot happen' }
a.repeated_combination(-2) { |combination| fail 'Cannot happen' }
```

---

Returns a new Enumerator if no block given:

```ruby
a = [0, 1, 2]
a.repeated_combination(2) # => #<Enumerator: [0, 1, 2]:repeated_combination(2)>
```

---

Raises an exception if <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a.repeated_combination(:foo) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### repeated_permutation

```
ary.repeated_permutation(n) { |element| ... } → self
ary.repeated_permutation(n) → new_enumerator
```
Calls the block with repeated permutations of length <tt>n</tt>
of the elements of <tt>ary</tt>;
returns <tt>self</tt>.  The order of repeated permutations is indeterminate.

Argument <tt>n</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When a block and a zero-valued argument <tt>n</tt> are given,
calls the block once with a new empty Array:

```ruby
a = [0, 1, 2]
a1 = a.repeated_permutation(0) { |permutation| p permutation }
a1.equal?(a) # => true # Identity check.
```

Output:

```
[]
```

If <tt>0 < n < ary.size</tt>,
calls the block with all <tt>n</tt>-tuple repeated permutations of <tt>ary</tt>:

```ruby
a = [0, 1, 2]
a.repeated_permutation(2) { |permutation| p permutation }
```

Output:

```
[0, 0]
[0, 1]
[0, 2]
[1, 0]
[1, 1]
[1, 2]
[2, 0]
[2, 1]
[2, 2]
```

```ruby

a = [0, 1, 2]
a.repeated_permutation(3) { |permutation| p permutation }
```

Output:

```
[0, 0, 0]
[0, 0, 1]
[0, 0, 2]
[0, 1, 0]
[0, 1, 1]
[0, 1, 2]
[0, 2, 0]
[0, 2, 1]
[0, 2, 2]
[1, 0, 0]
[1, 0, 1]
[1, 0, 2]
[1, 1, 0]
[1, 1, 1]
[1, 1, 2]
[1, 2, 0]
[1, 2, 1]
[1, 2, 2]
[2, 0, 0]
[2, 0, 1]
[2, 0, 2]
[2, 1, 0]
[2, 1, 1]
[2, 1, 2]
[2, 2, 0]
[2, 2, 1]
[2, 2, 2]
```

If <tt>n</tt> is negative, does not call the block:

```ruby
a = [0, 1, 2]
a.repeated_permutation(-1) { |permutation| fail 'Cannot happen' }
a.repeated_permutation(-2) { |permutation| fail 'Cannot happen' }
```

---

Returns a new Enumerator if no block given:

```ruby
a = [0, 1, 2]
a.repeated_permutation(2) # => #<Enumerator: [0, 1, 2]:repeated_permutation(2)>
```

---

Raises an exception if <tt>n</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a.repeated_permutation(:foo) { } # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### replace

```
ary.replace(other_array) → self
```

Replaces the content of <tt>ary</tt> with the content
of <tt>other_array</tt>; returns <tt>self</tt>.

Argument <tt>other_array</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to an Array.

---

Replaces the content of <tt>ary</tt> with the content of <tt>other_array</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.replace(['foo', :bar, 3])
a1 # => ["foo", :bar, 3]
a1.object_id == a.object_id # => true
```

Ignores the size of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.replace([]) # => []
a.replace([:foo, 'bar', baz = 2]) # => [:foo, "bar", 2]
```

---

Raises an exception if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a.replace(:foo) # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### reverse

```
ary.reverse → new_array
```

Returns a new Array whose elements are in reverse order from <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.reverse
a1 # => [2, "bar", :foo]
```

#### reverse!

```
ary.reverse → self
```

Returns <tt>self</tt> with elements in reverse order:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.reverse!
a1 # => [2, "bar", :foo]
a1.object_id == a.object_id # => true
```

#### reverse_each

```
ary.reverse_each { |element| ... } → self
ary.reverse_each → Enumerator
```

When a block given,
passes reverse_each successive <tt>element</tt> in <tt>ary</tt>
to the block in reverse order;  returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.reverse_each { |element|  puts "#{element.class} #{element}" }
a1.object_id == a.object_id # => true
```

Output:

```
Integer 2
String bar
Symbol foo
```

Allows <tt>ary</tt> to be modified during the iteration:

```ruby
a = [:foo, 'bar', baz = 2]
a.reverse_each { |element| puts element; a.clear if element.to_s.start_with?('b') }
a # => []
```

Output:

```
2
bar
```

---

When no block given, returns a new Enumerator:

```ruby
a = [:foo, 'bar', baz = 2]
e = a.reverse_each
e # => #<Enumerator: [:foo, "bar", 2]:reverse_each>
a1 = e.each { |element|  puts "#{element.class} #{element}" }
a1.object_id == a.object_id # => true
```

Output:

```
Integer 2
String bar
Symbol foo
```

#### rindex

```
ary.rindex(obj) → Integer or nil
ary.rindex { |element| ... } → Integer or nil
ary.rindex → new_enumerator
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
a.rindex { |element| element == :X } # => nil
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

#### rotate

```
ary.rotate → new_ary
ary.rotate(count) → new_ary
```

Returns a new Array formed from <tt>ary</tt> by rotating elements from one end to the other.

Argument <tt>count</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When no argument given, returns a new Array that is like <tt>ary</tt>,
except that the first element has been rotated to the last position:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a1 = a.rotate
a1 # => ["bar", 2, "bar", :foo]
```

---

When given a non-negative argument <tt>count</tt>,
returns a new Array with <tt>count</tt> elements rotated from the beginning to the end:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate(2)
a1 # => [2, :foo, "bar"]
```

If <tt>count</tt> is large, uses <tt>count % ary.size</tt> as the count:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate(20)
a1 # => [2, :foo, "bar"]
```

If <tt>count</tt> is <tt>0</tt>, returns an unmodified copy of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate(0)
a1 # => [:foo, "bar", 2]
```

---

When given a negative argument <tt>count</tt>, rotates in the opposite direction, from end to beginning:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate(-2)
a1 # => ["bar", 2, :foo]
```

If <tt>count</tt> is small, uses <tt>count % ary.size<</tt> as the count:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate(-5)
a1 # => ["bar", 2, :foo]
```

---

Raises an exception if <tt>count</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer) 
```

#### rotate!

```
ary.rotate! → self
ary.rotate(count) → self
```

Rotates <tt>ary</tt> by moving elements from one end to the other; returns <tt>self</tt>.

Argument <tt>count</tt>, if given, must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

When no argument given, rotates the first element to the last position:

```ruby
a = [:foo, 'bar', baz = 2, 'bar']
a1 = a.rotate!
a1 # => ["bar", 2, "bar", :foo]
a1.object_id == a.object_id # => true
```

---

When given a non-negative argument <tt>count</tt>,
rotates <tt>count</tt> elements from the beginning to the end:

```ruby
a = [:foo, 'bar', baz = 2]
a.rotate!(2)
a # => [2, :foo, "bar"]
```

If <tt>count</tt> is large, uses <tt>count % ary.size</tt> as the count:

```ruby
a = [:foo, 'bar', baz = 2]
a.rotate!(20)
a # => [2, :foo, "bar"]
```

If <tt>count</tt> is <tt>0</tt>, returns <tt>self</tt> unmodified:

```ruby
a = [:foo, 'bar', baz = 2]
a.rotate!(0)
a # => [:foo, "bar", 2]
```

---

When given a negative argument <tt>count</tt>, rotates in the opposite direction, from end to beginning:

```ruby
a = [:foo, 'bar', baz = 2]
a.rotate!(-2)
a # => ["bar", 2, :foo]
```

If <tt>count</tt> is small, uses <tt>count % ary.size<</tt> as the count:

```ruby
a = [:foo, 'bar', baz = 2]
a.rotate!(-5)
a # => ["bar", 2, :foo]
```

---

Raises an exception if <tt>count</tt> is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.rotate!(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer) 
```

#### select

```
ary.select { |element| ... } → new_array
ary.select → new_enumeration
```

Returns a new Array containing those elements of <tt>ary</tt>
for which the block returns a truthy value:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a1 = a.select { |element| element.to_s.start_with?('b') }
a1 # => ["bar", :bam]
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a.select # => #<Enumerator: [:foo, "bar", 2, :bam]:select>
```

#### select!

```
ary.select! { |element| ... } → new_array
ary.select! → new_enumeration
```

Removes from <tt>ary</tt> those elements for which the block
returns <tt>false</tt> or <tt>nil</tt>; returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a1 = a.select! { |element| element.to_s.start_with?('b') }
a1 # => ["bar", :bam]
a1.object_id == a.object_id 
```

---

Returns a new Enumerator if no block given:

```ruby
a = [:foo, 'bar', baz = 2, :bam]
a.select! # => #<Enumerator: [:foo, "bar", 2, :bam]:select!>
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

Raises an exception if given a single argument that is not an Integer
or a Range:

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

#### slice!

```
ary.slice!(n) → obj or nil
ary.slice!(start, length) → new_array or nil
ary.slice!(range) → new_array or nil
```

Removes and returns elements from <tt>ary</tt>.

---

When the only argument is an Integer <tt>n</tt>,
removes and returns the <tt>n</tt>th element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(1) # => "bar"
a # => [:foo, 2]
```

If <tt>n</tt> is negative,
counts relative to the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(-1) # => 2
a # => [:foo, "bar"]
```

If <tt>n</tt> is out of range,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(50) # => nil
a.slice!(-50) # => nil
a # => [:foo, "bar", 2]
```

---

When the only arguments are Integers <tt>start</tt> and <tt>length</tt>,
removes <tt>length</tt> elements from <tt>ary</tt> beginning at index <tt>start</tt>; returns the deleted objects in a new array:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(0, 2) # => [:foo, "bar"]
a # => [2]
```

If <tt>start + length > ary.length</tt>,
removes and returns <tt>ary.size - start</tt> elements:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(1, 50) # => ["bar", 2]
a # => [:foo]
```

If <tt>start == a.size</tt> and <tt>length >= 0</tt>,
returns a new empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(a.size, 0) # => []
a.slice!(a.size, 50) # => []
a # => [:foo, "bar", 2]
```

If <tt>length</tt> is negative,
returns <tt>nil</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(2, -1) # => nil
a.slice!(1, -2) # => nil
a # => [:foo, "bar", 2]
```

---

When the only argument is a Range object <rng>,
treats <tt>rng.min</tt> as <tt>start</tt> above
and <tt>rng.size</tt> as <tt>length</tt> above:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(1..2) # => ["bar", 2]
a # => [:foo]
```

If <tt>rng.start == a.size</tt>,
returns a new empty Array:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(a.size..0) # => []
a.slice!(a.size..50) # => []
a.slice!(a.size..-1) # => []
a.slice!(a.size..-50) # => []
a # => [:foo, "bar", 2]
```

If <tt>rng.end</tt> is negative,
calculates the end index from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(0..-2) # => [:foo, "bar"]
a # => [2]
```

If <tt>rng.start</tt> is negative,
calculates the start index from the end of <tt>ary</tt>:
```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(-2..2) # => ["bar", 2]
a # => [:foo]
```

---

Raises an exception if given a single argument that is not an Integer
or a Range:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(:foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

Raises an exception if given two arguments that are not both Integers:

```ruby
a = [:foo, 'bar', baz = 2]
a.slice!(:foo, 3) # Raises TypeError (no implicit conversion of Symbol into Integer)
a.slice!(1, :bar) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### sort

```
ary.sort → new_array
ary.sort { |a, b| ... } → new_array
```

Returns a new Array whose elements are those from <tt>ary</tt>, sorted.

---

With no block, compares elements using operator <tt><=></tt>:

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a1 = a.sort
a1 # => ["a", "b", "c", "d", "e"]
```

---

With a block, compares elements using the block's return value.

For each element pair <tt>a</tt> and <tt>b</tt>, the block should return an integer:
* Negative when <tt>b</tt> is to follow <tt>a</tt>.
* Zero when <tt>a</tt> and <tt>b</tt> are equivalent.
* Positive when <tt>a</tt> is to follow <tt>b</tt>.

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a1 = a.sort { |a, b| a <=> b }
a1 # => ["a", "b", "c", "d", "e"]
a2 = a.sort { |a, b| b <=> a }
a2 # => ["e", "d", "c", "b", "a"]
```

When the block returns <tt>0</tt>, the order for <tt>a</tt> and <tt>b</tt>
is indeterminate, and may be unstable:

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a1 = a.sort { |a, b| 0 }
a1 # => ["c", "e", "b", "d", "a"]
```

---

Raises an exception if the block returns a non-Integer:

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a1 = a.sort { |a, b| :foo } # Raises ArgumentError (comparison of Symbol with 0 failed)
```

#### sort!

```
ary.sort! → self
ary.sort! { |a, b| ... } → self
```

Returns <tt>self</tt> with its elements sorted.

---

With no block, compares elements using operator <tt><=></tt>:

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a.sort!
a # => ["a", "b", "c", "d", "e"]
```

---

With a block, compares elements using the block's return value.

For each element pair <tt>a</tt> and <tt>b</tt>, the block should return an integer:
* Negative when <tt>b</tt> is to follow <tt>a</tt>.
* Zero when <tt>a</tt> and <tt>b</tt> are equivalent.
* Positive when <tt>a</tt> is to follow <tt>b</tt>.

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a.sort! { |a, b| a <=> b }
a # => ["a", "b", "c", "d", "e"]
a.sort! { |a, b| b <=> a }
a # => ["e", "d", "c", "b", "a"]
```

When the block returns <tt>0</tt>, the order for <tt>a</tt> and <tt>b</tt>
is indeterminate, and may be unstable:

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a.sort! { |a, b| 0 }
a # => ["d", "e", "c", "a", "b"]
```

---

Raises an exception if the block returns a non-Integer:

```ruby
a = 'abcde'.split('').shuffle
a # => ["e", "b", "d", "a", "c"]
a1 = a.sort! { |a, b| :foo } # Raises ArgumentError (comparison of Symbol with 0 failed)
```

#### sort_by!

```
ary.sort_by! { |element| ... } → self
ary.sort_by! → new_enumerator
```

Sorts <tt>ary</tt> using an ordering determined by the block;
returns <tt>self</tt>.

Calls the block with each successive element;
sorts elements based on the collection of values
returned from the block.

For duplicates returned by the block,
the ordering is indeterminate, and may be unstable.

This example sorts strings based on their sizes:

```ruby
a = ['aaaa', 'bbb', 'cc', 'd']
a.sort_by! { |element| element.size }
a # => ["d", "cc", "bbb", "aaaa"]
````
---

Returns a new Enumerator if no block given:

```ruby
a = ['aaaa', 'bbb', 'cc', 'd']
a.sort_by! # => #<Enumerator: ["aaaa", "bbb", "cc", "d"]:sort_by!>
```

#### to_a

```
ary.to_a → self or new_array
```

When <tt>ary</tt> is an Array, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.to_a
a1.object_id == a.object_id # => true
```

When <tt>ary</tt> is a subclass of Array,
returns a new Array containing the elements of <tt>ary</tt>:

```ruby
class SubArray < Array; end
a = SubArray.new([:foo, 'bar', baz = 2])
a1 = a.to_a
a1.class # => Array
a1 == a # => true
```

#### to_ary

```

ary.to_ary → self or new_array
```

When <tt>ary</tt> is an Array, returns <tt>self</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a1 = a.to_ary
a1.object_id == a.object_id # => true
```

When <tt>ary</tt> is a subclass of Array,
returns a new Array containing the elements of <tt>ary</tt>:

```ruby
class SubArray < Array; end
a = SubArray.new([:foo, 'bar', baz = 2])
a1 = a.to_ary
a1.class # => Array
a1 == a # => true
```

#### to_h

```
ary.to_h → new_hash
ary.to_h {|element| ... }  → new_hash
```

Returns a new Hash formed from <tt>ary</tt>.

---

When no block given,
returns a new Hash wherein each 2-element Array object in <tt>ary</tt>
becomes a key-value pair in the returned Hash:

```ruby
[].to_h # => {}
a = [[:foo, 0], [:bar, 1], [:baz, 2]]
h = a.to_h
h # => {:foo=>0, :bar=>1, :baz=>2}
```

---

Raises an exception if any element is not an Array:

```ruby
[:foo].to_h # Raises TypeError (wrong element type Symbol at 0 (expected array))
```

Raises an exception if any element is an Array not of size 2:

```ruby
[[:foo]].to_h # Raises ArgumentError (wrong array length at 0 (expected 2, was 1))
```

Raises an exception if any <tt>ary[n].first</tt> would be an
[invalid hash key](../../Hash/api/markdown.md#invalid-hash-keys):

```ruby
[[BasicObject.new, 0]].to_h # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### to_s

```
ary.to_s → new_string
```

Returns a new String formed by calling method <tt>inspect</tt>
on each element in <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.to_s  # => "[:foo, \"bar\", 2]"
```

Raises an exception if any element lacks instance method <tt>inspect</tt>:

```ruby
a = [:foo, 'bar', baz = 2, BasicObject.new]
a.to_s  # Raises NoMethodError (undefined method `inspect' for #<BasicObject:0x0000000006b69d28>)
```

#### transpose

```
ary.transpose → new_array
```

Transposes the rows and columns in an array of arrays.

Each element in <tt>ary</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to an Array.

```ruby
a = [[:a0, :a1], [:b0, :b1], [:c0, :c1]]
a.transpose # => [[:a0, :b0, :c0], [:a1, :b1, :c1]]
```

---

Raises an exception if an element in <tt>ary</tt> is not an
 [Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
a = [[:a0, :a1], [:b0, :b1], :foo]
a.transpose # Raises TypeError (no implicit conversion of Symbol into Array)
```

Raises an exception if the sizes of the sub-arrays differ:

```ruby
a = [[:a0, :a1], [:b0, :b1], [:c0, :c1, :c2]]
a.transpose # Raises IndexError (element size differs (3 should be 2))
```

#### union

```
ary.union(*other_arrays) → new_array
```

Each argument must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects).

---

Returns a new Array that is the union of <tt>ary</tt> and all arguments,
with duplicates removed and order preserved;
compares using <tt>eql?</tt>:

```ruby
[0, 1, 2, 3].union([4, 5], [6, 7]) # => [0, 1, 2, 3, 4, 5, 6, 7]
[0, 1, 1].union([2, 1], [3, 1]) # => [0, 1, 2, 3]
[0, 1, 2, 3].union([3, 2], [1, 0]) # => [0, 1, 2, 3]
```

---

Raises an exeption if any argument is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[].union([], :foo) # Raises TypeError (no implicit conversion of Symbol into Array)
```

#### uniq

```
ary.uniq → new_array
ary.uniq { |element| ... } → new_array
```

Returns a new Array containing those elements from <tt>ary</tt>
that are not duplicates, the first occurrence always being retained.

With no block, identifies duplicates
by comparing elements with <tt>eql?</tt>:

```ruby
a = [0, 0, 1, 1, 2, 2]
a.uniq # => [0, 1, 2]
```

With a block, calls the block for each element;
identifies duplicates
by comparing block return values with <tt>eql?</tt>:

```ruby
a = ['a', 'aa', 'aaa', 'b', 'bb', 'bbb']
a.uniq { |element| element.size } # => ["a", "aa", "aaa"]
```

---

Raises an exception if any element does not respond to
instance method <tt>hash</tt>:

```ruby
[BasicObject.new, BasicObject.new].uniq # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

Raises an exception if the block returns an element
that does not respond to method <tt>hash</tt>:

```ruby
[0, 1].uniq { |x| BasicObject.new } # Raises NoMethodError (undefined method `hash' for #<BasicObject:0x0000000006baaf08>)
```

#### uniq!

```
ary.uniq! → self or nil
ary.uniq! { |element| ... } → self or nil
```

Removes duplicate elements from <tt>ary</tt>,
the first occurrence always being retained.
Returns <tt>self</tt> if any elements removed,
<tt>nil</tt> otherwise.

With no block, removes duplicate elements identified
by comparing elements with <tt>eql?</tt>:

```ruby
a = [0, 0, 1, 1, 2, 2]
a1 = a.uniq!
a1 # => [0, 1, 2]
a1.equal?(a) # => true # Identity check
[0, 1, 2].uniq! # => nil
```

With a block, calls the block for each element;
removes duplicate elements identified
by comparing block return values with <tt>eql?</tt>:

```ruby
a = ['a', 'aa', 'aaa', 'b', 'bb', 'bbb']
a.uniq! { |element| element.size } # => ["a", "aa", "aaa"]
a = ['a', 'aa', 'aaa']
a.uniq! { |element| element.size } # => nil
```

---

Raises an exception if any element does not respond to
instance method <tt>hash</tt>:

```ruby
[BasicObject.new, BasicObject.new].uniq! # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

Raises an exception if the block returns an element
that does not respond to method <tt>hash</tt>:

```ruby
[0, 1].uniq! { |x| BasicObject.new } # Raises NoMethodError (undefined method `hash' for #<BasicObject:0x0000000006baaf08>)
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

#### values_at

```
ary.values_at(*indexes) → new_array
```

Returns a new Array whose elements are the <tt>ary</tt> elements
at the given <tt>indexes</tt>.

Each index given in <tt>*indexes</tt> must be an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects).

---

For each positive index <tt>index</tt>,
returns the element at offset <tt>index</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(0, 2) # => [:foo, 2]
```

The given <tt>*indexes</tt> may be in any order, and may repeat:

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(2, 0, 1, 0, 2) # => [2, :foo, "bar", :foo, 2]
```

Assigns <tt>nil></tt> for an index that is too large:

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(0, 3, 1, 3) # => [:foo, nil, "bar", nil]
```

---

For each negative index, counts backward from the end of <tt>ary</tt>:

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(-1, -3) # => [:foo, 2] # => [2, :foo]
```

The given <tt>*indexes</tt> may have a mixture of signs:

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(0, -2, 1, -1) # => [:foo, "bar", "bar", 2]
```

Assigns <tt>nil</tt> for an index that is too small:

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(0, -5, 1, -6, 2) # => [:foo, nil, "bar", nil, 2]
```

Raises an exception if any index is not an
[Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
a = [:foo, 'bar', baz = 2]
a.values_at(0, :foo) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### zip

```
ary.zip(*other_arrays) → new_array
ary.zip(*other_arrays) { |other_array| ... } → nil
```

Each object in <tt>*other_arrays</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects),
which will be converted to an Array.

---

With no block, returns a new Array of size <tt>ary.size</tt>
whose elements are Arrays.

Each nested array <tt>new_array[n]</tt>
is of size <tt>other_arrays.size+1</tt>, and contains:
* The <tt>n</tt>th element of <tt>ary</tt>.
* The <tt>n</tt>th element of each of the <tt>*other_arrays</tt>.

If all the arrays are the same size:

```ruby
a = [:a0, :a1, :a2, :a3]
b = [:b0, :b1, :b2, :b3]
c = [:c0, :c1, :c2, :c3]
d = a.zip(b, c)
d # => [[:a0, :b0, :c0], [:a1, :b1, :c1], [:a2, :b2, :c2], [:a3, :b3, :c3]]
```

If any array in <tt>other_arrays</tt> is smaller than <tt>ary</tt>,
fills to <tt>ary.size</tt> with <tt>nil</tt>:

```ruby
a = [:a0, :a1, :a2, :a3]
b = [:b0, :b1, :b2]
c = [:c0, :c1]
d = a.zip(b, c)
d # => [[:a0, :b0, :c0], [:a1, :b1, :c1], [:a2, :b2, nil], [:a3, nil, nil]]
```

If any array in <tt>other_arrays</tt> is larger than <tt>ary</tt>,
its trailing elements are ignored:

```ruby
a = [:a0, :a1, :a2, :a3]
b = [:b0, :b1, :b2, :b3, :b4]
c = [:c0, :c1, :c2, :c3, :c4, :c5]
d = a.zip(b, c)
d 
```

---

When a block given, calls the block
with each of the sub-arrays (formed as above);
returns <tt>nil</tt>

```ruby
a = [:a0, :a1, :a2, :a3]
b = [:b0, :b1, :b2, :b3]
c = [:c0, :c1, :c2, :c3]
a.zip(b, c) { |sub_array| p sub_array} # => nil
```

Output:

```
[:a0, :b0, :c0]
[:a1, :b1, :c1]
[:a2, :b2, :c2]
[:a3, :b3, :c3]
```

---

Raises an exception if any of the given <tt>*other_arrays</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
a = [:a0, :a1, :a2, :a3]
b = [:b0, :b1, :b2, :b3]
c = [:c0, :c1, :c2, :c3]
d = a.zip(b, c, :foo) # Raises TypeError (wrong argument type Symbol (must respond to :each))
```

#### | (Union)

```
ary | other_array → new_array
```

Argument <tt>other_array</tt> must be an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects).

---

Returns the union of <tt>ary</tt> and <tt>other_array</tt>,
with duplicates removed and order preserved:

```ruby
[0, 1] | [2, 3] # => [0, 1, 2, 3]
[0, 1, 1] | [2, 2, 3] # => [0, 1, 2, 3]
[0, 1, 2] | [3, 2, 1, 0] # => [0, 1, 2, 3]
```

---

Raises an exception if <tt>other_array</tt> is not an
[Array-convertible object](../../../doc/convertibles.md#array-convertible-objects):

```ruby
[] | :foo # Raises TypeError (no implicit conversion of Symbol into Array)
```
