## Hash API

### Contents
- [Hash Data Syntax](#hash-data-syntax)
- [Default Values](#default-values)
  - [Default Value](#default-value)
  - [Default Proc](#default-proc)
- [Entry Order](#entry-order)
- [Hash Keys](#hash-keys)
  - [Invalid Hash Keys](#invalid-hash-keys)
  - [Modifying an Active Hash Key](#modifying-an-active-hash-key)
  - [User-Defined Hash Keys](#user-defined-hash-keys)
- [Public Class Methods](#public-class-methods)
  - [::[] (Literal)](#-literal)
  - [::new](#new)
  - [::try_convert](#try_convert)
- [Public Instance Methods](#public-instance-methods)
  - [< (Proper Subset)](#-proper-subset)
  - [<= (Subset)](#-subset)
  - [== (Equality)](#-equality)
  - [> (Proper Superset)](#-proper-superset)
  - [>= (Superset)](#-superset)
  - [[] (Index)](#-index)
  - [[]= (Assignment)](#-assignment)
  - [assoc](#assoc)
  - [clear](#clear)
  - [compact](#compact)
  - [compact!](#compact-1)
  - [compare_by_identity](#compare_by_identity)
  - [compare_by_identity?](#compare_by_identity-1)
  - [default](#default)
  - [default=](#default-1)
  - [default_proc](#default_proc)
  - [default_proc=](#default_proc-1)
  - [delete](#delete)
  - [delete_if](#delete_if)
  - [dig](#dig)
  - [each](#each)
  - [each_key](#each_key)
  - [each_pair](#each_pair)
  - [each_value](#each_value)
  - [empty?](#empty)
  - [eql?](#eql)
  - [fetch](#fetch)
  - [fetch_values](#fetch_values)
  - [filter](#filter)
  - [filter!](#filter-1)
  - [flatten](#flatten)
  - [has_key?](#has_key)
  - [has_value?](#has_value)
  - [hash](#hash)
  - [include?](#include)
  - [inspect](#inspect)
  - [invert](#invert)
  - [keep_if](#keep_if)
  - [key](#key)
  - [key?](#key-1)
  - [keys](#keys)
  - [length](#length)
  - [member?](#member)
  - [merge](#merge)
  - [merge!](#merge-1)
  - [rassoc](#rassoc)
  - [rehash](#rehash)
  - [reject](#reject)
  - [reject!](#reject-1)
  - [replace](#replace)
  - [select](#select)
  - [select!](#select-1)
  - [shift](#shift)
  - [size](#size)
  - [slice](#slice)
  - [store](#store)
  - [to_a](#to_a)
  - [to_h](#to_h)
  - [to_hash](#to_hash)
  - [to_proc](#to_proc)
  - [to_s](#to_s)
  - [transform_keys](#transform_keys)
  - [transform_keys!](#transform_keys-1)
  - [transform_values](#transform_values)
  - [transform_values!](#transform_values-1)
  - [update](#update)
  - [value?](#value)
  - [values](#values)
  - [values_at](#values_at)

### Hash Data Syntax

Until its version 1.9,
Ruby supported only the "hash rocket" syntax for Hash data:

```ruby
h = {:foo => 0, :bar => 1, :baz => 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

(The "hash rocket" is <tt>=></tt>,
sometimes in other languages called the "fat comma.")


Beginning with version 1.9,
you can write a Hash key that's a Symbol in a JSON-style syntax,
where each bareword becomes a Symbol:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

You can also use Strings instead of barewords:

```ruby
h = {'foo': 0, 'bar': 1, 'baz': 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

And you can mix the styles:

```ruby
h = {foo: 0, :bar => 1, 'baz': 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

But it's an error to try the JSON-style syntax
for a key that's not a bareword or a String:

```ruby
h = {0: 'zero'} # Raises SyntaxError (syntax error, unexpected ':', expecting =>)

```

### Default Values

For a key that is not found, method <tt>[]</tt> returns a default value
determined by:

* Its default proc, if the default proc is not <tt>nil</tt>.
* Its default value, otherwise.

#### Default Value

A Hash object's default value is relevant only
when its default proc is <tt>nil</tt>.
(Initially, both are <tt>nil</tt>.)

You can retrieve the default value with method #default:

```ruby
h = Hash.new
h.default # => nil
```

You can initialize the default value by passing an argument to method Hash.new:

```ruby
h = Hash.new(false)
h.default # => false
```

You can update the default value with method #default=:

```ruby
h = Hash.new
h.default # => nil
h.default = false
h.default # => false
```

When the default proc is <tt>nil</tt>,
method #[] returns the value of method #default:

```ruby
h = Hash.new
h.default_proc # => nil
h.default # => nil
h[:nosuch] # => nil
h.default = false
h[:nosuch] # => false
```

For certain kinds of default values, the default value can be modified thus:

```ruby
h = Hash.new('Foo')
h[:nosuch] # => "Foo"
h[:nosuch].upcase! # => "FOO"
h[:nosuch] # => "FOO"
h.default = [0, 1]
h[:nosuch] # => [0, 1]
h[:nosuch].reverse! # => [1, 0]
h[:nosuch] # => [1, 0]
```

#### Default Proc

When the default proc for a Hash is set (i.e., not <tt>nil</tt>),
the default value returned by method #[] is determined by the default proc alone.

You can retrieve the default proc with method #default_proc:

```ruby
h = Hash.new
h.default_proc # => nil
```

You can initialize the default proc by by calling Hash.new with a block:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

You can update the default proc with method #default_proc=:

```ruby
h = Hash.new
h.default_proc # => nil
h.default_proc = proc { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

When the default proc is set (i.e., not <tt>nil</tt>)
and method #[] is called with with a non-existent key,
method #[] calls the default proc with both the Hash object itself and the missing key,
then returns the proc's return value:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h[:nosuch] # => "Default value for nosuch"
```

And the default value is ignored:

```ruby
h.default = false
h[:nosuch] # => "Default value for nosuch"
```

Note that in the example above no entry for key +:nosuch+ is created:

```ruby
h.include?(:nosuch) # => false
```

However, the proc itself can add a new entry:

```ruby
h = Hash.new { |hash, key| hash[key] = "Subsequent value for #{key}"; "First value for #{key}" }
h.include?(:nosuch) # => false
h[:nosuch] # => "First value for nosuch"
h.include?(:nosuch) # => true
h[:nosuch] # => "Subsequent value for nosuch"
h[:nosuch] # => "Subsequent value for nosuch"
```

You can set the default proc to <tt>nil</tt>, which restores control to the default value:
 
```ruby
h.default_proc = nil
h.default = false
h[:nosuch] # => false
````

### Entry Order

A Hash object presents its entries in the order of their creation. This is seen in:

* Iterative methods such as <tt>each</tt>, <tt>each_key</tt>, <tt>each_pair</tt>, <tt>each_value</tt>.
* Other order-sensitive methods such as <tt>shift</tt>, <tt>keys</tt>, <tt>values</tt>.
* The String returned by method <tt>inspect</tt>.

A new Hash has its initial ordering per the given entries:

```ruby
h = Hash[foo: 0, bar: 1]
h # => {:foo=>0, :bar=>1}
```

New entries are added at the end:

```ruby
h[:baz] = 2
h # => {:foo=>0, :bar=>1, :baz=>2}
```

Updating a value does not affect the order:

```ruby
h[:baz] = 3
h # => {:foo=>0, :bar=>1, :baz=>3}
```

But re-creating a deleted entry can affect the order:

```ruby
h.delete(:foo)
h[:foo] = 5
h # => {:bar=>1, :baz=>3, :foo=>5}
```

### Hash Keys

#### Invalid Hash Keys

An object that lacks method <tt>hash</tt>
cannot be a Hash key:

```ruby
{BasicObject.new => 0} # Raises NoMethodError (undefined method `hash' for #<BasicObject>)
```

#### Modifying an Active Hash Key

Modifying a Hash key while it is in use damages the hash's index.

This Hash has keys that are Arrays:

```ruby
a0 = [ :foo, :bar ]
a1 = [ :baz, :bat ]
h = {a0 => 0, a1 => 1}
h.include?(a0) # => true
h[a0] # => 0
a0.hash # => 110002110
```

 Modifying array element <tt>a0[0]</tt> changes its hash value:
```ruby
a0[0] = :bam
a0.hash # => 1069447059
```

And damages the hash's index:

```ruby
h.include?(a0) # => false
h[a0] # => nil
```

You can repair the hash index using method <tt>rehash</tt>:

```ruby
h.rehash # => {[:bam, :bar]=>0, [:baz, :bat]=>1}
h.include?(a0) # => true
h[a0] # => 0
```

A String key is always safe.
That's because an unfrozen String
passed as a key will be replaced by a duplicated and frozen String:

```ruby
s = 'foo'
s.frozen? # => false
h = {s => 0}
first_key = h.keys.first
first_key.frozen? # => true
first_key.equal?(s) # => false
```

#### User-Defined Hash Keys

I can't improve on the discussion of user-defined
objects as keys  over at [ruby-doc.org](https://ruby-doc.org/core-2.7.0/Hash.html#class-Hash-label-Hash+Keys) (and don't want to steal from it).

### Public Class Methods

#### ::[] (Literal)

```
Hash[] → new_empty_hash
Hash[*keys_and_values] → new_hash
Hash[[*2_element_arrays]] → new_hash
Hash[hashable_object] → new_hash
```

Returns a new Hash object populated with the given objects, if any.

The initial default value and default proc are set to <tt>nil</tt> (see [Default Values](#default-values)):

```ruby
h = Hash[]
h # => {}
h.class # => Hash
h.default # => nil
h.default_proc # => nil
```

When an even number of arguments is given,
returns a new hash wherein each successive pair of arguments has become a key-value entry:

```ruby
Hash[] # => {}
Hash[:foo, 0, :bar, 1] # => {:foo=>0, :bar=>1}
```

When the only argument is an array of 2-element arrays, returns a new hash wherein each 2-element array forms a key-value entry:

```ruby
Hash[ [ [:foo, 0], [:bar, 1] ] ] # => {:foo=>0, :bar=>1}
```

When the only argument is a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects),
converts the object and returns the resulting Hash:

```ruby
class Foo
  def to_hash
    {foo: 0, bar: 1}
  end
end
Hash[Foo.new] # => {:foo=>0, :bar=>1}
```

Raises an exception if the argument count is 1, but the argument is not an array of 2-element arrays or an object that is convertible to a hash:

```ruby
Hash[:foo] # Raises ArgumentError (odd number of arguments for Hash)
Hash[ [ [:foo, 0, 1] ] ] # Raises ArgumentError (invalid number of elements (3 for 1..2))
```

Raises an exception if the argument count is odd and greater than 1:

```ruby
Hash[0, 1, 2] # Raises ArgumentError (odd number of arguments for Hash)
```

Raises an exception if the argument is an array containing an element
that is not a 2-element array:

```ruby
Hash[ [ :foo ] ] # Raises ArgumentError (wrong element type Symbol at 0 (expected array))
```

Raises an exception if the argument is an array containing an element
that is an array of size different from 2:

```ruby
Hash[ [ [0, 1, 2] ] ] # Raises ArgumentError (invalid number of elements (3 for 1..2))
```

Raises an exception if any proposed key is not a valid key:

```ruby
Hash[:foo, 0, BasicObject.new, 1] # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
Hash[ [ [:foo, 0], [BasicObject.new, 1] ] ] # Raises NoMethodError (undefined method `hash' for #<BasicObject:>
```

Raises an exception if the only argument is an object whose instance method to_hash takes arguments:

```ruby
class Foo; def to_hash(x) {}; end; end
Hash[Foo.new] # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

Raises an exception if the only argument is an object whose instance method to_hash does not return a Hash object:

```ruby
class Foo
  def to_hash
   :foo
  end
end
Hash[Foo.new] # Raises TypeError (can't convert Foo to Hash (Foo#to_hash gives Symbol))
```

#### ::new

```
Hash.new → new_empty_hash
Hash.new(default_value) → new_empty_hash
Hash.new { |hash, key| ... } → new_empty_hash
```

Returns a new empty Hash object.

The initial default value and initial default proc for the new hash depend on which form above was used.  See [Default Values](#default-values).

If neither argument nor block given, initializes both the default value and the default proc to <tt>nil</tt>:

```ruby
h = Hash.new
h # => {}
h.class # => Hash
h.default # => nil
h.default_proc # => nil
h[:nosuch] # => nil
```

If argument <tt>default_value</tt> given but no block given,
initializes the default value to the given <tt>default_value</tt>
and the default proc to <tt>nil</tt>:

```ruby
h = Hash.new(false)
h # => {}
h.default # => false
h.default_proc # => nil
h[:nosuch] # => false
```

If block given but no argument given, stores the block as the default proc,
and sets the default value to <tt>nil</tt>:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h # => {}
h.default # => nil
h.default_proc.class # => Proc
h[:nosuch] # => "Default value for nosuch"
```

Raises an exception if both default_value and a block are given:
```ruby
Hash.new(0) { } # Raises ArgumentError (wrong number of arguments (given 1, expected 0))
```


#### ::try_convert

```
Hash.try_convert(obj) → new_hash or nil
```

Returns the Hash object created by calling
<tt>obj.to_hash</tt>.

```ruby
class HashableSet < Set
  def to_hash
    h = {}
    self.map { |item| h[item] = nil}
    h
  end
end
hs = HashableSet.new([:foo, :bar, :baz])
Hash.try_convert(hs) # => {:foo=>nil, :bar=>nil, :baz=>nil}
```

Returns <tt>nil</tt> unless <tt>obj.respond_to?(:to_hash)</tt>:

```ruby
s = 'foo'
s.respond_to?(:to_hash) # => false
Hash.try_convert(s) # => nil
```

Raises an exception unless <tt>obj.to_hash</tt> returns a Hash object:

```ruby
class BadToHash
  def to_hash
    1
  end
end
hs = BadToHash.new
Hash.try_convert(hs) # Raises TypeError (can't convert BadToHash to Hash (BadToHash#to_hash gives Integer))
```

### Public Instance Methods

(Class Hash includes module [Enumerable](https://ruby-doc.org/core-2.7.0/Enumerable.html); there are dozens more instance methods from that module.)

#### < (Proper Subset)

```
hash < other_hash → true or false
```

Returns <tt>true</tt> if <tt>hash</tt> is a proper subset of <tt>other_hash</tt>,
 <tt>false</tt> otherwise:

```ruby
h1 = {foo: 0, bar: 1}
h2 = {foo: 0, bar: 1, baz: 2}
h1 < h2 # => true
h2 < h1 # => false
h1 < h1 # => false
```

Raises an exception if <tt>other_hash</tt>
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h < 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### <= (Subset)

```
hash <= other_hash → true or false
```

Returns <tt>true</tt> if <tt>hash</tt> is a subset of <tt>other_hash</tt>,
 <tt>false</tt> otherwise:

```ruby
h1 = {foo: 0, bar: 1}
h2 = {foo: 0, bar: 1, baz: 2}
h1 <= h2 # => true
h2 <= h1 # => false
h1 <= h1 # => true
```

Raises an exception if <tt>other_hash</tt>
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects)

```ruby
h = {}
h <= 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```


#### == (Equality)

```
hash == other_hash → true or false
```

Returns <tt>true</tt> if all of the following are true:
* <tt>other_hash</tt> is a Hash object.
* <tt>hash</tt> and <tt>other_hash</tt> have the same
  keys (regardless of order).
* For each key <tt>key</tt>, <tt>hash[key] == other_hash[key]</tt>.

Otherwise, returns <tt>false</tt>.

Equal:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1, baz: 2}
h1 == h2 # => true
h3 = {baz: 2, bar: 1, foo: 0}
h1 == h3 # => true
```

Not equal because of class:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h1 == 1 # false
```

Not equal because of different keys:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1, zab: 2}
h1 == h2 # => false
```

Not equal because of different values:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1, baz: 3}
h1 == h2 # => false
```

#### > (Proper Superset)

```
hash > other_hash → true or false
```

Returns <tt>true</tt> if <tt>hash</tt> is a proper superset of <tt>other_hash</tt>,
 <tt>false</tt> otherwise:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1}
h1 > h2 # => true
h2 > h1 # => false
h1 > h1 # => false
```

Raises an exception if <tt>other_hash</tt>
 is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h > 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### >= (Superset)

```
hash >= other_hash → true or false
```

Returns <tt>true</tt> if <tt>hash</tt> is a superset of <tt>other_hash</tt>,
 <tt>false</tt> otherwise:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1}
h1 >= h2 # => true
h2 >= h1 # => false
h1 >= h1 # => true
```

Raises an exception if <tt>other_hash</tt>
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h >= 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### [] (Index)

```
hash[key] → value
```

Returns the value associated with key <tt>key</tt>, if found:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h[:foo] # => 0
```

If <tt>key</tt> not found, returns the default value (see [Default Values](#default-values)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h[:nosuch] # => nil
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys))):

```ruby
h = {}
h[BasicObject.new] # Raises NoMethodError (undefined method `to_s' for #<BasicObject:>)
```
#### []= (Assignment)

```
hash[key] = value → value
```

Associates <tt>value</tt> with <tt>key</tt>, and returns <tt>value</tt>.

If key <tt>key</tt> exists, replaces its value;
the ordering is not affected (see [Entry Order](#entry-order)):

```ruby
h = {foo: 0, bar: 1}
h[:foo] = 2 # => 2
h # => {:foo=>2, :bar=>1}
```

If key <tt>key</tt> does not exist, adds the key and value;
the new entry is last in the order (see [Entry Order](#entry-order)):

```ruby
h = {foo: 0, bar: 1}
h[:baz] = 2 # => 2
h # => {:foo=>0, :bar=>1, :baz=>2}
```

Raises an exception if the key is invalid
(see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1}
h[BasicObject.new] = 2 # Raises NoMethodError (undefined method `hash' for #<BasicObject>)
```

#### assoc

```
hash.assoc(key) → new_array or nil
```

If key <tt>key</tt> is found, returns a 2-element Array
containing that key and its value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.assoc(:bar) # => [:bar, 1]
```

Returns <tt>nil</tt> if key <tt>key</tt> is not found:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.assoc(:nosuch)
```

Raises an exception if the key is invalid
(see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.assoc(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject>)
```

#### clear

```
hash.clear → self
```

Removes all hash entries, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.clear # => {}
h1.object_id == h.object_id # => true
```

#### compact

```
hash.compact → new_hash
```

Returns a copy of <tt>self</tt> with all <tt>nil</tt>-valued entries removed:

```ruby
h = {foo: 0, bar: nil, baz: 2, bat: nil}
h1 = h.compact
h1 # => {:foo=>0, :baz=>2}
h1.object_id == h.object_id # => false
```

#### compact!

```
hash.compact! → self or nil
```

Returns <tt>self</tt> with all <tt>nil</tt>-valued entries removed:

```ruby
h = {foo: 0, bar: nil, baz: 2, bat: nil}
h1 = h.compact!
h1 # => {:foo=>0, :baz=>2}
h1.object_id == h.object_id # => true
```

Returns <tt>nil</tt> if no entries were removed:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.compact! # => nil
h # => {:foo=>0, :bar=>1, :baz=>2}
```

#### compare_by_identity

```
hash.compare_by_identity → self
```

Sets <tt>self</tt> to consider only identity in comparing keys, returning <tt>self</tt>;
two keys are considered the same only if they are the same object.

By default, these two keys are considered the same, and therefore overwrite:

```ruby
s0 = 'x'
s1 = 'x'
s0.object_id == s1.object_id # => false
h = {}
h.compare_by_identity? # => false
h[s0] = 0
h[s1] = 1
h # => {"x"=>1}
```

After calling <tt>compare_by_identity</tt>, the keys are considered different,
and therefore do not overwrite:

```ruby
s0 = 'x'
s1 = 'x'
s0.object_id == s1.object_id # => false
h = {}
h1 = h.compare_by_identity # => {}
h1.object_id == h.object_id # => true
h.compare_by_identity? # => true
h[s0] = 0
h[s1] = 1
h # => {"x"=>0, "x"=>1}
```

#### compare_by_identity?

```
hash.compare_by_identity? → true or false
```

Returns <tt>true</tt> if <compare_by_identity> has been called, <tt>false</tt> otherwise:

```ruby
h = {}
h.compare_by_identity? # false
h.compare_by_identity
h.compare_by_identity? # true
```

#### default

```
hash.default → value
hash.default(key) → value
```

With no argument, returns the current default value:

```ruby
h = {}
h.default # => nil
h.default = false
h.default # => false
```

With <tt>key</tt> given, returns the default value for <tt>key</tt>,
regardless of whether that key exists:

```ruby
h = {}
h.default[:nosuch] # => nil
```

The returned value will be determined either by the default proc or by the default value.
See [Default Values](#default-values).

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = []
h.default(BasicObject.new) # Raises NoMethodError (undefined method `to_s' for #<BasicObject:>)
```

#### default=

```
hash.default = value → value
```

Sets the default value to <tt>value</tt>, returning <tt>value</tt>:

```ruby
h = {}
h.default # => nil
h.default = false # => false
h.default # => false
```

See [Default Values](#default-values).


#### default_proc

```
hash.default_proc → proc or nil
```

Returns the default proc:

```ruby
h = {}
h.default_proc # => nil
h.default_proc = proc { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

See [Default Values](#default-values).

#### default_proc=

```
hash.default_proc = proc → proc
```

Sets the default proc to <tt>proc</tt>:

```ruby
h = {}
h.default_proc # => nil
h.default_proc = proc { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
h.default_proc = nil
h.default_proc # => nil
```

See [Default Values](#default-values).

Raises an exception if <tt>proc</tt> is not a <tt>Proc</tt> object or <tt>nil</tt>:

```ruby
h = {}
h.default_proc = 0 # Raises TypeError (wrong default_proc type Integer (expected Proc))
```

#### delete

```
hash.delete(key) → value
hash.delete(key) { |key| ... } → value
```

If no block is given and key <tt>key</tt> is found, deletes its entry and returns the associated value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(:bar) # => 1
h # => {:foo=>0, :baz=>2}
```

If no block given and key <tt>key</tt> is not found, returns <tt>nil</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(:nosuch) # => nil
h # => {:foo=>0, :bar=>1, :baz=>2}
```

If a is block given and key <tt>key</tt> is found, ignores the block, deletes the entry,
and returns the associated value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(:baz) { |key| fail 'Will never happen'} # => 2
h # => {:foo=>0, :bar=>1}
```

If a block is given and key <tt>key</tt> is not found,
calls the block and returns the block's return value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(:nosuch) { |key| "Key #{key} not found" } # => "Key nosuch not found"
h # => {:foo=>0, :bar=>1, :baz=>2}
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### delete_if

```
hash.delete_if { |key, value| ... } → self
hash.delete_if → new_enumerator
```

Calls the block with each key-value pair,
deletes each entry for which the block returns a truthy value,
and returns <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.delete_if { |key, value| value > 0 }
h1 # => {:foo=>0}
h1.object_id == h.object_id # => true
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.delete_if # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:delete_if>
h1 = e.each { |key, value| value > 0 }
h1 # => {:foo=>0}
h1.object_id == h.object_id # => true
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete_if { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### dig

```
hash.dig(*keys) → value
```

Returns the value for a specified object in nested objects.

For nested Hash objects:
* For each key in <tt>*keys</tt>, calls method <tt>dig</tt> on a receiver.
* The first receiver is <tt>self</tt>.
* Each successive receiver is the value returned by the previous call to <tt>dig</tt>.
* The value finally returned is the value returned by the last call to <tt>dig</tt>.

Examples:

```ruby
h = {foo: 0}
h.dig(:foo) # => 0
```

```ruby
h = {foo: {bar: 1}}
h.dig(:foo, :bar) # => 1
```

```ruby
h = {foo: {bar: {baz: 2}}}
h.dig(:foo, :bar, :baz) # => 2
```

Returns <tt>nil</tt> if any key is not found:

```ruby
h = { foo: {bar: {baz: 2}}}
h.dig(:foo, :nosuch) # => nil
```

The nested objects may include any that respond to <tt>:dig</tt>,
which includes instances of:
* Hash
* Array
* Struct
* OpenStruct
* CSV::Table
* CSV::Row

Example:

```ruby
h = {foo: {bar: [:a, :b, :c]}}
h.dig(:foo, :bar, 2) # => :c
```

Raises an exception if any given key is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h.dig(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

Raises an exception if any receiver does not respond to <tt>dig</tt>:

```ruby
h = { foo: 1 }
h.dig(:foo, 1) # Raises TypeError: Integer does not have #dig method
```

#### each

```
hash.each { |key, value| ... } → self
hash.each → new_enumerator
```

Calls the given block with each key-value pair, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.each { |key, value| puts "#{key}: #{value}"}
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
foo: 0
bar: 1
baz: 2
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.each # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each>
h1 = e.each { |key, value| puts "#{key}: #{value}"}
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
foo: 0
bar: 1
baz: 2
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

<tt>#each</tt> is an alias for <tt>#each_pair</tt>.

#### each_key

```
hash.each_key { |key| ... } → self
hash.each_key → new_enumerator
```

Calls the given block with each key, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.each_key { |key| puts key }
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
foo
bar
baz
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.each_key # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each_key>
h1 = e.each { |key| puts key }
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
foo
bar
baz
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each_key { |key| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### each_pair

```
hash.each_pair { |key, value| ... } → self
hash.each_pair → new_enumerator
```

Calls the given block with each key-value pair, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.each_pair { |key, value| puts "#{key}: #{value}"}
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
foo: 0
bar: 1
baz: 2
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.each_pair # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each_pair>
h1 = e.each { |key, value| puts "#{key}: #{value}"}
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
foo: 0
bar: 1
baz: 2
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each_pair { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### each_value

```
hash.each_value { |value| ... } → self
hash.each_value → new_enumerator
```

Calls the given block with each value, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.each_value { |value| puts value }
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
0
1
2
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.each_value # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each_value>
h1 = e.each { |value| puts value }
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Output:

```
0
1
2
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each_value { |value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### empty?

```
hash.empty? → true or false
```

Returns <tt>true</tt> if there are no hash entries, <tt>false</tt> otherwise:

```ruby
{}.empty? # => true
{foo: 0, bar: 1, baz: 2}.empty? # => false
```

#### eql?

```
hash.eql? other_hash → true or false
```

Returns <tt>true</tt> if all of the following are true:
* <tt>other_hash</tt> is a Hash object.
* <tt>h</tt> and <tt>other_hash</tt> have the same
  keys (regardless of order).
* For each key <tt>key</tt>, <tt>h[key] eql? other_hash[key]</tt>.

Otherwise, returns <tt>false</tt>.

Equal:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1, baz: 2}
h1.eql? h2 # => true
h3 = {baz: 2, bar: 1, foo: 0}
h1.eql? h3 # => true
```

Not equal because of class:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h1.eql? 1 # false
```

Not equal because of different keys:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1, zab: 2}
h1.eql? h2 # => false
```

Not equal because of different values:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {foo: 0, bar: 1, baz: 3}
h1.eql? h2 # => false
```

#### fetch

```
hash.fetch(key) → value
hash.fetch(key , default) → value
hash.fetch(key) { |key| ... } → value
```

Returns the value for key <tt>key</tt>.

If neither <tt>default</tt> nor a block given:
* If key <tt>key</tt> found, returns its associated value.
* Otherwise, raises an exception:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch(:bar) # => 1
h.fetch(:nosuch) # Raises KeyError (key not found: :nosuch)
```

If <tt>default</tt> is given, but no block:
* If key <tt>key</tt> found, returns its associated value.
* Otherwise, returns the given <tt>default</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch(:bar, :default) # => 1
h.fetch(:nosuch, :default) # => :default
```

If a block is given, but no <tt>default</tt>:
* If key <tt>key</tt> found, returns its associated value.
* Otherwise, calls the block with <tt>key</tt>, and returns the block's return value.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch(:bar) { |key| fail 'Ignored'} # => 1
h.fetch(:nosuch) { |key| "Value for #{key}"} # => "Value for nosuch"
```

If both <tt>default</tt> and a block are given:
* Ignores <tt>default</tt> and issues a warning 'block supersedes default value argument'.
* If key <tt>key</tt> found, returns its associated value.
* Otherwise, calls the block with <tt>key</tt>, and   returns the block's return value.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch(:bar, :default) { |key| fail 'Ignored'} # => 1
h.fetch(:nosuch, :default) { |key| "Value for #{key}"} # => "Value for nosuch"
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### fetch_values

```
hash.fetch_values(*keys) → new_array
hash.fetch_values(*keys) { |key| ... } → new_array
```

Returns a new Array containing the values associated with the given keys <tt>*keys</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch_values(:baz, :foo) # => [2, 0]
```

Returns a new empty Array if no arguments given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch_values # => []
```

When a block given, calls the block with each missing key,
treating the block's return value as the value for that key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
values = h.fetch_values(:bar, :foo, :bad, :bam) { |key| key.to_s}
values # => [1, 0, "bad", "bam"]
```
Raises an exception if any given key is not found:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch_values(:baz, :nosuch) # Raises KeyError (key not found: :nosuch)
```

Raises an exception if any given key is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.fetch_values(:baz, BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### filter

```
hash.filter { |key, value| ... } → new_hash
hash.filter → new_enumerator
```

Returns a new Hash object consisting of the entries for which the block returns a truthy value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.filter { |key, value| key.start_with?('b') }
h1 # => {:bar=>1, :baz=>2}
h1.object_id == h.object_id # => false
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.filter # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:filter>
h1 = e.each { |key, value| key.start_with?('b') }
h1 # => {:bar=>1, :baz=>2}
h1.object_id == h.object_id # => false
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.filter { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

<tt>#filter</tt> is an alias for <tt>#select</tt>.

#### filter!

```
hash.filter! { |key, value| ... } → self or nil
hash.filter! → new_enumerator
```

Deletes each hash entry for which the block returns <tt>nil</tt> or <tt>false</tt>, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.filter! { |key, value| key.start_with?('b') }
h1 # => {:bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Returns <tt>nil</tt> if no entries were deleted:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.filter! { |key, value| true } # => nil
h # => {:foo=>0, :bar=>1, :baz=>2}
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.filter! # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:filter!>
h1 = e.each { |key, value| key.start_with?('b') }
h1 # => {:bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.filter! { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

<tt>#filter!</tt> is an alias for <tt>#select!</tt>.

#### flatten

```
hash.flatten(level = 1) → new_array
```

Accepts optional argument <tt>level</tt>,
which must be an [Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects)

Returns a new Array object wherein each key and each value of from <tt>self</tt> is an array element:

```ruby
h = {foo: 0, bar: [:bat, 3], baz: 2}
h.flatten # => [:foo, 0, :bar, [:bat, 3], :baz, 2]
```

Takes the level of recursive flattening from <tt>level</tt>:

```ruby
h = {foo: 0, bar: [:bat, [:baz, [:bat, ]]]}
h.flatten(1) # => [:foo, 0, :bar, [:bat, [:baz, [:bat]]]]
h.flatten(2) # => [:foo, 0, :bar, :bat, [:baz, [:bat]]]
h.flatten(3) # => [:foo, 0, :bar, :bat, :baz, [:bat]]
h.flatten(4) # => [:foo, 0, :bar, :bat, :baz, :bat]
```

When <tt>level</tt> is negative, flattens all levels:

```ruby
h = {foo: 0, bar: [:bat, [:baz, [:bat, ]]]}
h.flatten(-1) # => [:foo, 0, :bar, :bat, :baz, :bat]
h.flatten(-2) # => [:foo, 0, :bar, :bat, :baz, :bat]
```

When <tt>level</tt> is <tt>0</tt>, returns the equivalent of #to_a :

```ruby
h = {foo: 0, bar: [:bat, 3], baz: 2}
h.flatten(0) # => [[:foo, 0], [:bar, [:bat, 3]], [:baz, 2]]
h.flatten(0) == h.to_a # => true
```

Converts <tt>level</tt> to an Integer object if necessary and possible:

```ruby
h = {foo: 0, bar: [:bat, 3], baz: 2}
h.flatten(Float(1.1)) # => [:foo, 0, :bar, [:bat, 3], :baz, 2]
h.flatten(Complex(2, 0)) # => [:foo, 0, :bar, :bat, 3, :baz, 2]
```

Raises an exception if <tt>level</tt> is not
an [Integer-convertible object](../../../doc/convertibles.md#integer-convertible-objects):

```ruby
h = {foo: 0, bar: [:bat, 3], baz: 2}
h.flatten(:nosuch) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### has_key?

```
hash.has_key?(key) → true or false
```

Returns <tt>true</tt> if <tt>key</tt> is a key in the hash, otherwise <tt>false</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.has_key?(:bar) # => true
h.has_key?(:nosuch) # => false
h.has_key?(BasicObject.new) # false 
```

#### has_value?

```
hash.has_value?(value) → true or false
```

Returns <tt>true</tt> if <tt>value</tt> is a value in the hash, otherwise <tt>false</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.has_value?(1) # => true
h.has_value?(123) # => false
```

#### hash

```
hash.hash → an_integer
```

Returns the Integer hash value for the hash:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h1.hash.class # => Integer
```

Two Hash objects have the same hash value if their content is the same:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {baz: 2, bar: 1, foo: 0}
h2.hash == h1.hash # => true
h2.eql? h1 # => true
```

#### include?

```
hash.include?(key) → true or false
```

Returns <tt>true</tt> if <tt>key</tt> is a key in the hash, otherwise <tt>false</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.include?(:bar) # => true
h.include?(:nosuch) # => false
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h.include?(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

<tt>#include?</tt> is an alias for <tt>#has_key?</tt>.

#### inspect

```
hash.inspect → new_string
```

Returns a new String showing the hash entries:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.inspect # => "{:foo=>0, :bar=>1, :baz=>2}"
```

#### invert

```
hash.invert → new_hash
```

Returns a new Hash object with the each key-value pair reversed:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.invert
h1 # => {0=>:foo, 1=>:bar, 2=>:baz}
h1.object_id == h.object_id # => false
```

Overwrites any repeated new keys:

```ruby
h = {foo: 0, bar: 0, baz: 0}
h.invert # => {0=>:baz}
```

Raises an exception if any value cannot be a key:

```ruby
h = {foo: 0, bar: 1, baz: BasicObject.new}
h.invert # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### keep_if

```
hash.keep_if { |key, value| ... } → self
hash.keep_if → new_enumerator
```

Calls the block for each key-value pair,
retains the entry if the block returns a truthy value,
deletes the entry otherwise,
and returns <tt>self</tt>.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.keep_if { |key, value| key.start_with?('b') }
h1 # => {:bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.keep_if # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:keep_if>
h1 = e.each { |key, value| key.start_with?('b') }
h1 # => {:bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.keep_if { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### key

```
hash.key(value) → key or nil
```

Returns the key for the first-found entry with value <tt>value</tt>:

```ruby
h = {foo: 0, bar: 2, baz: 2}
h.key(0) # => :foo
h.key(2) # => :bar
```

Returns <tt>nil</tt> if so such value is found:

```ruby
h = {}
h.key(0) # => nil
```

#### key?

```
hash.key?(key) → true or false
```

Returns <tt>true</tt> if <tt>key</tt> is a key in the hash, <tt>false</tt> otherwise:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.key?(:foo) # => true
h.key?(:nosuch) # => false
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h.key?(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

<tt>#key?</tt> is an alias for <tt>#has_key?</tt>.

#### keys

```
hash.keys → new_array
```

Returns a new Array containing all keys in <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.keys # => [:foo, :bar, :baz]
```

#### length

```
hash.length → an_integer
```

Returns the count the entries in <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.length # => 3
```

<tt>#length</tt> is an alias for <tt>#size</tt>.

#### member?

```
hash.member?(key) → true or false
```

Returns <tt>true</tt> if <tt>key</tt> is a key in the hash, <tt>false</tt> otherwise:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.member?(:foo) # => true
h.member?(:nosuch) # => false
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h.member?(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

<tt>#member?</tt> is an alias for <tt>#has_key?</tt>.

#### merge

```
hash.merge → new_copy_of_hash
hash.merge(*other_hashes) → new_hash
hash.merge(*other_hashes) { |key, old_value, new_value| ... } → new_hash
```

With arguments and no block:
* Returns a new Hash object that is the merge of <tt>self</tt>
and each given hash.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* Each duplicate-key entry's value overwrites the previous value.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge(h1, h2)
h3 # => {:foo=>0, :bar=>4, :baz=>2, :bat=>6, :bam=>5}
h3.object_id == h.object_id # false
```

With arguments and a block:
* Returns a new Hash object that is the merge of <tt>self</tt> and each given hash.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* For each duplicate key:
  * Calls the block with the key and the old and new values.
  * The block's return value becomes the new value for the entry.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge(h1, h2) { |key, old_value, new_value| old_value + new_value }
h3 # => {:foo=>0, :bar=>5, :baz=>2, :bat=>9, :bam=>5}
h3.object_id == h.object_id # => false
```

Ignores an attempt in the block to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge(h1, h2) { |key, old_value, new_value| h[:new_key] = 10 }
h3 # => {:foo=>0, :bar=>10, :baz=>2, :bat=>10, :bam=>5}
h3.object_id == h.object_id # => false
```

With no arguments:
* Returns a copy of <tt>self</tt>.
* The block, if given, is ignored.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.merge
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => false
h2 = h.merge { |key, old_value, new_value| fail 'Cannot happen' }
h2 # => {:foo=>0, :bar=>1, :baz=>2}
h2.object_id == h.object_id # => false
```

Raises an exception if any given argument
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h.merge(1) # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### merge!

```
hash.merge! → self
hash.merge!(*other_hashes) → self
hash.merge!(*other_hashes) { |key, old_value, new_value| ... } → self
```

With arguments and no block:
* Returns <tt>self</tt>, after the given hashes are merged into it.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* Each duplicate-key entry's value overwrites the previous value.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge!(h1, h2) # => {:foo=>0, :bar=>4, :baz=>2, :bat=>6, :bam=>5}
h3.object_id == h.object_id # => true
```

With arguments and a block:
* Returns <tt>self</tt>, after the given hashes are merged.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* For each duplicate key:
  * Calls the block with the key and the old and new values.
  * The block's return value becomes the new value for the entry.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge!(h1, h2) { |key, old_value, new_value| old_value + new_value }
h3 # => {:foo=>0, :bar=>5, :baz=>2, :bat=>9, :bam=>5}
h3.object_id == h.object_id # => true
```

Allows the block to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge!(h1, h2) { |key, old_value, new_value| h[:new_key] = 10 }
h3 # => {:foo=>0, :bar=>10, :baz=>2, :bat=>10, :new_key=>10, :bam=>5}
h3.object_id == h.object_id # => true
```

With no arguments:
* Returns <tt>self</tt>, unmodified.
* The block, if given, is ignored.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.merge # => {:foo=>0, :bar=>1, :baz=>2}
h1 = h.merge! { |key, old_value, new_value| fail 'Cannot happen' }
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Raises an exception if any given argument
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h.merge!(1) # Raises TypeError (no implicit conversion of Integer into Hash)
```

<tt>#merge!</tt> is an alias for <tt>#update</tt>.

#### rassoc

```
hash.rassoc(value) → new_array or nil
```

Returns a new 2-element Array consisting of the key and value
of the first-found entry whose value is <tt>==</tt> to <tt>value</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 1}
h.rassoc(1) # => [:bar, 1]
```

Returns <tt>nil</tt> if no such value found:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.rassoc(3) # => nil
```

#### rehash

```
hash.rehash → self
```

Returns <tt>self</tt> after rebuilding its index
from the current hash value for each key.

This Hash has keys that are Arrays:

```ruby
a0 = [ :foo, :bar ]
a1 = [ :baz, :bat ]
h = { a0 => 0, a1 => 1 }
h.include?(a0) # => true
h[a0] # => 0
a0.hash # => 110002110
```

Modifying array element <tt>a0[0]</tt> changes its hash value:
```ruby
a0[0] = :bam
a0.hash # => 1069447059
```

And damages the hash's index:

```ruby
h.include?(a0) # => false
h[a0] # => nil
```

You can repair the hash index using method <tt>rehash</tt>:

```ruby
h1 = h.rehash # => {[:bam, :bar]=>0, [:baz, :bat]=>1}
h.include?(a0) # => true
h[a0] # => 0
h1.object_id == h.object_id # => true
```

Raises an exception if called while hash iteration in progress:

```ruby
h = {foo: 0, bar: 1, baz: 1}
h.each { |key, value| h.rehash } # Raises RuntimeError (rehash during iteration)
```

#### reject

```
hash.reject { |key, value| ...} → new_hash
hash.reject → new_enumerator 
```

Returns a new Hash object whose entries are all those from <tt>self</tt>
for which the block returns <tt>false</tt> or <tt>nil</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.reject { |key, value| key.start_with?('b') }
h1 # => {:foo=>0}
h1.object_id == h.object_id # => false
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.reject # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:reject>
h1 = e.each { |key, value| key.start_with?('b') }
h1 # => {:foo=>0}
h1.object_id == h.object_id # => false
```

#### reject!

```
hash.reject! { |key, value| ... } → self or nil
hash.reject! → new_enumerator
```

Returns <tt>self</tt>, whose remaining entries are all those
for which the block returns <tt>false</tt> or <tt>nil</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.reject! { |key, value| value < 2 }
h1 # => {:baz=>2}
h1.object_id == h.object_id # => true
```

Returns <tt>nil</tt> if no entries were removed:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.reject! { |key, value| value > 2 }
h
```
Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.reject! # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:reject!>
h1 = e.each { |key, value| key.start_with?('b') }
h1 # => {:foo=>0}
h1.object_id == h.object_id # => true
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.reject! { |key, value| h[:new_Key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### replace

```
hash.replace(other_hash) → self
```

Replaces the entire contents of <tt>self</tt> with the contents of <tt>other_hash</tt>;
returns <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.replace({bat: 3, bam: 4})
h1 # => {:bat=>3, :bam=>4}
h1.object_id == h.object_id # => true
```

Raises an exception if <tt>other_hash</tt>
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h.replace(:not_a_hash) # Raises TypeError (no implicit conversion of Symbol into Hash)

```
#### select

```
hash.select { |key, value| ... } → new_hash
hash.select → new_enumerator
```

Returns a new Hash object whose entries are those for which the block returns a truthy value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.select { |key, value| value < 2 }
h1 # => {:foo=>0, :bar=>1}
h1.object_id == h.object_id # => false
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.select # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:select>
h1 = e.each { |key, value| value < 2 }
h1 # => {:foo=>0, :bar=>1}
h1.object_id == h.object_id # => false
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.select { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### select!

```
hash.select! { |key, value| ... } → self or nil
hash.select! → new_enumerator
```

Returns <tt>self</tt>, whose entries are those for which the block returns a truthy value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.select! { |key, value| value < 2 }
h # => {:foo=>0, :bar=>1}
h1.object_id == h.object_id # => true
```

Returns <tt>nil</tt> if no entries were removed:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.select! { |key, value| value < 3} # => nil 
h # => {:foo=>0, :bar=>1, :baz=>2}
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.select! # => # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:select!>
h1 = e.each { |key, value| value < 2 }
h1 # => {:foo=>0, :bar=>1}
h1.object_id == h.object_id # => true
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.select! { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### shift

```
hash.shift → [key, value] or default_value
```

Removes the first entry from <tt>self</tt> (see [Entry Order](#entry-order)),
returning a 2-element Array containing the removed key and its value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.shift # => [:foo, 0]
h # => {:bar=>1, :baz=>2}
```

Returns the default value if the hash is empty (see [Default Values](#default-values)):

```ruby
h = {}
h.shift # => nil
```

#### size

```
hash.size → an_integer
```

Returns the count of hash entries:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.size # => 3
h.delete(:foo) # => 0
h.size # => 2
```

#### slice

```
hash.slice(*keys) → new_hash
```

Returns a new Hash object containing the entries for the given <tt>*keys</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.slice(:baz, :foo)
h1 # => {:baz=>2, :foo=>0}
h1.object_id == h.object_id # => false
```

Raises an exception if any given key is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.slice(:foo, BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### store

```
hash.store(key, value) → value
```

Creates or updates an entry with the given <tt>key</tt> and <tt>value</tt>, returning <tt>value</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.store(:bat, 3) # => 3
h.store(:foo, 4) # => 5
h # => {:foo=>4, :bar=>1, :baz=>2, :bat=>3}
```

Raises an exception if <tt>key</tt> is invalid  (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.store(BasicObject.new, 3) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### to_a

```
hash.to_a → new_array
```

Returns a new Array of 2-element Array objects;
each nested Array contains the key and value for a hash entry:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.to_a # => [[:foo, 0], [:bar, 1], [:baz, 2]]
```

#### to_h

```
hash.to_h → self or new_hash
hash.to_h { |key, value| ... } → new_hash
```

For an instance of Hash, returns <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_h
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

For a subclass of Hash, returns a new Hash containing the content of <tt>self</tt>:

```ruby
class H < Hash; end
h = H[foo: 0, bar: 1, baz: 2]
h # => {:foo=>0, :bar=>1, :baz=>2}
h.class # => H
h1 = h.to_h
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.class # => Hash
```

When a block is given, returns a new Hash object
whose content is based on the block;
the block should return a 2-element Array object
specifying the key-value pair to be included in the returned Array:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_h { |key, value| [value, key] }
h1 # => {0=>:foo, 1=>:bar, 2=>:baz}
```

Raises an exception if the block does not return an Array:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_h { |key, value| :array } # Raises TypeError (wrong element type Symbol (expected array))
```

Raises an exception if the block returns an Array of size different from 2:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_h { |key, value| [0, 1, 2] } # Raises ArgumentError (element has wrong array length (expected 2, was 3))
```

Raises an exception if the block returns an invalid key (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_h { |key, value| [BasicObject.new, 0] } # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.to_h { |key, value| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### to_hash

```
hash.to_hash → self
```

Returns <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_hash
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

#### to_proc

```
hash.to_proc → proc
```

Returns a Proc object that maps a key to its value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
proc = h.to_proc
proc.class # => Proc
proc.call(:foo) # => 0
proc.call(:bar) # => 1
proc.call(:nosuch) # => nil
[:foo, :bar, :nosuch].map(&h) # => [0, 1, nil]
```

#### to_s

```
hash.to_s → new_string
```

Returns a new String showing the hash entries:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.to_s # => "{:foo=>0, :bar=>1, :baz=>2}"
```
<tt>#to_s</tt> is an alias for <tt>#inspect</tt>.

#### transform_keys

```
hash.transform_keys { |key| ... } → new_hash
hash.transform_keys → new_enumerator
```

Returns a new Hash object of size <tt>self.size</tt>;
each entry has:'
* A key provided by the block.
* The value from <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_keys { |key| key.to_s }
h1 # => {"foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => false
```

Overwrites values for duplicate keys:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_keys { |key| :bat }
h1 # => {:bat=>2}
h1.object_id == h.object_id # => false
```

Returns a new Enumerator if no block given:
```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_keys # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:transform_keys>
h1 = e.each { |key| key.to_s }
h1 # => {"foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => false
```

Raises an exception if the block returns an invalid key (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_keys { |key| BasicObject.new } # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

Raises an exception if the block attempts to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_keys { |key| h[:new_key] = 3 } # Raises RuntimeError (can't add a new key into hash during iteration)
```

#### transform_keys!

```
hash.transform_keys! { |key| ... } → self
hash.transform_keys! → new_enumerator
```

Returns <tt>self</tt> with new keys provided by the block:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_keys! { |key| key.to_s }
h1 # => {"foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => true
```

Overwrites values for duplicate keys:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_keys! { |key| :bat }
h1 # => {:bat=>2}
h1.object_id == h.object_id # => true
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_keys! # => #<Enumerator: {"foo"=>0, "bar"=>1, "baz"=>2}:transform_keys!>
h1 = e.each { |key| key.to_s }
h1 # => {"foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => true
```

Raises an exception if the block returns an invalid key (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_keys! { |key| BasicObject.new } # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

Allows the block to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_keys! { |key| h[:new_key] = key.to_s }
h1 # => {:new_key=>"baz", "foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => true
```

#### transform_values

```
hash.transform_values { |value| ... } → new_hash
hash.transform_values → new_enumerator
```

Returns a new Hash object of size <tt>self.size</tt>;
each entry has:
* A key from <tt>self</tt>.
* The value provided by the block.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_values { |value| value * 100}
h1 # => {:foo=>0, :bar=>100, :baz=>200}
h1.object_id == h.object_id # => false
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_values # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:transform_values>
h1 = e.each { |value| value * 100}
h1 # => {:foo=>0, :bar=>100, :baz=>200}
h1.object_id == h.object_id # => false
```

Ignores an attempt in the block to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_values { |key| h[:new_key] = 3 }
h1 # => {:foo=>3, :bar=>3, :baz=>3}
```

#### transform_values!

```
hash.transform_values! { |value| ... } → self
hash.transform_values! → new_enumerator
```

Returns <tt>self</tt>, whose keys are unchanged,
and whose values are determined by the given block.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_values! { |value| value * 100}
h1 # => {:foo=>0, :bar=>100, :baz=>200}
h1.object_id == h.object_id # => true
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_values! # => #<Enumerator: {:foo=>0, :bar=>100, :baz=>200}:transform_values!>
h1 = e.each { |value| value * 100}
h1 # => {:foo=>0, :bar=>100, :baz=>200}
h1.object_id == h.object_id # => true
```

Allows the block to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_values! { |key| h[:new_key] = 3 }
h1 # => {:foo=>3, :bar=>3, :baz=>3, :new_key=>3}
```

#### update

```
hash.update → self
hash.update(*other_hashes) → self
hash.update(*other_hashes) { |key, old_value, new_value| ... } → self
```

With arguments and no block:
* Returns <tt>self</tt>, after the given hashes are merged into it.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* Each duplicate-key entry's value overwrites the previous value.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.update(h1, h2)
h3 # => {:foo=>0, :bar=>4, :baz=>2, :bat=>6, :bam=>5}
h3.object_id == h.object_id # => true
```

With arguments and a block:
* Returns <tt>self</tt>, after the given hashes are merged.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* For each duplicate key:
  * Calls the block with the key and the old and new values.
  * The block's return value becomes the new value for the entry.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.update(h1, h2) { |key, old_value, new_value| old_value + new_value }
h3 # => {:foo=>0, :bar=>5, :baz=>2, :bat=>9, :bam=>5}
h3.object_id == h.object_id # => true
```

With no arguments:
* Returns <tt>self</tt>, unmodified.
* The block, if given, is ignored.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.merge # => {:foo=>0, :bar=>1, :baz=>2}
h1 = h.update { |key, old_value, new_value| fail 'Cannot happen' }
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

Raises an exception if any given argument
is not a [Hash-convertible object](../../../doc/convertibles.md#hash-convertible-objects):

```ruby
h = {}
h.merge(:foo) # Raises TypeError (no implicit conversion of Symbol into Hash)
```
Allows the block to add a new key:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.update(h1, h2) { |key, old_value, new_value| h[:new_key] = 3 }
h3 # => {:foo=>0, :bar=>3, :baz=>2, :bat=>3, :new_key=>3, :bam=>5}
h3.object_id == h.object_id # => true
```

#### value?

```
hash.value?(value) → true or false
```

Returns <tt>true</tt> if <tt>value</tt> is a value in the hash,
<tt>false</tt> otherwise:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.value?(0) # => true
h.value?(3) # => false
```

<tt>#value?</tt> is an alias for <tt>#has_value?</tt>.

#### values

```
hash.values → new_array
```

Returns a new Array containing all values in <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values # => [0, 1, 2]
```

#### values_at

```
hash.values_at(*keys) → new_array
```

Returns a new Array containing values for the given <tt>*keys</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values_at(:foo, :baz) # => [0, 2]
```

Returns an empty Array if no arguments given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values_at # => []
```

Raises an exception if any given key is invalid
(see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values_at(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:0x0000000006c13800>)
```
