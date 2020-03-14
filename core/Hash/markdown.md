## Hash

A Hash is a dictionary-like collection of unique _keys_,
each of which has an associated _value_.

A Hash has certain similarities to an Array, but:
* An Array index is always an Integer.
* A Hash key can be (almost) any object.

### Contents
- [Common Uses](#common-uses)
- [Creating a Hash](#creating-a-hash)
  - [Constructor Hash.new](#constructor-hashnew)
  - [Hash Literal](#hash-literal)
  - [Hash Implicit Form](#hash-implicit-form)
- [Getting a Hash Value](#getting-a-hash-value)
- [Setting a Hash Value](#setting-a-hash-value)
- [Deleting a Hash Value](#deleting-a-hash-value)
- [Default Values](#default-values)
  - [Default Value](#default-value)
  - [Default Proc](#default-proc)
- [Entry Order](#entry-order)
- [Hash Keys](#hash-keys)
  - [Invalid Hash Keys](#invalid-hash-keys)
  - [Modifying an Active Hash Key](#modifying-an-active-hash-key)
  - [User-Defined Hash Keys](#user-defined-hash-keys)
  - [Hash-Convertible Arguments](#hash-convertible-arguments)
- [Public Class Methods](#public-class-methods)
  - [::[]](#-)
  - [::new](#-new)
  - [::try_convert](#-try_convert)
- [Public Instance Methods](#public-instance-methods)
  - [<](#)
  - [<=](#-1)
  - [==](#-2)
  - [>](#-3)
  - [>=](#-4)
  - [[]](#-5)
  - [[]=](#-6)
  - [assoc](#assoc)
  - [clear](#clear)
  - [compact](#compact)
  - [compact!](#compact-)
  - [compare_by_identiry](#compare_by_identiry)
  - [compare_by_identity?](#compare_by_identity)
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
  - [filter!](#filter-)
  - [flatten](#flatten)
  - [has_key?](#has_key)
  - [has_value?](#has_value)
  - [hash](#hash-1)
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
  - [merge!](#merge-)
  - [rassoc](#rassoc)
  - [rehash](#rehash)
  - [reject](#reject)
  - [reject!](#reject-)
  - [replace](#replace)
  - [select](#select)
  - [select!](#select-)
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
  - [transform_keys!](#transform_keys-)
  - [transform_values](#transform_values)
  - [transform_values!](#transform_values-)
  - [update](#update)
  - [value](#value)
  - [values](#values)
  - [values_at](#values_at)

### Common Uses

You can use a Hash to give names to objects:

```ruby
matz = {:name => 'Matz', :language => 'Ruby' }
matz # => {:name=>"Matz", :language=>"Ruby"}
```

You can also use a Hash to give names to method arguments:

```ruby
class Dev
  def initialize(hash)
    @name = hash[:name]
    @language = hash[:language]
  end
end
matz = Dev.new({:name => 'Matz', :language => 'Ruby'})
matz # => #<Dev: @name="Matz", @language="Ruby">
```

Note: when the last argument in a method call is a Hash,
the curly braces may be omitted:

```ruby
matz = Dev.new(:name => 'Matz', :language => 'Ruby')
matz # => #<Dev: @name="Matz", @language="Ruby">
```

### Creating a Hash

Here are three ways to create a Hash:
* Constructor method: Hash.new.
* Literal method: Hash.[].
* Implicit Form: {}.

#### Constructor Hash.new

You can create a Hash by using the constructor method, Hash.new.

Create an empty Hash:

```ruby
h = Hash.new
p h
p h.class
```

#### Hash Literal

You can create a Hash by using its literal method, Hash.[].

Create an empty Hash:

```ruby
h = Hash[]
h # => {}
```

Create a Hash with initial entries:

```ruby
h = Hash[:foo => 0, :bar => 1, :baz => 2]
h # => {:foo=>0, :bar=>1, :baz=>2}
```

For any key that's to be a Symbol, there's this shorthand:

```ruby
h = Hash[foo: 0, bar: 1, baz: 2]
h # => {:foo=>0, :bar=>1, :baz=>2}
```

And you can mix the two styles:

```ruby
h = Hash[:foo => 0, bar: 1, :baz => 2]
h # => {:foo=>0, :bar=>1, :baz=>2}
```

#### Hash Implicit Form

You can create a Hash by using its implicit form (curly braces).

Create an empty Hash:

```ruby
h = {}
h # => {}
```

Create a Hash with initial entries:

```ruby
h = {:foo => 0, :bar => 1, :baz => 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

For any key that's to be a Symbol, there's this shorthand:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

And you can mix the two styles:

```ruby
h = {foo: 0, :bar => 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```


### Getting a Hash Value

The simplest way to get a Hash value (instance method <tt>[]</tt>):

```ruby
h[:foo] # => 0
```

### Setting a Hash Value

The simplest way to create or update a Hash value (instance method <tt>[]=</tt>):

```ruby
h[:bat] = 3 # => 3
h # => {:foo=>0, :bar=>1, :baz=>2, :bat=>3}
h[:foo] = 4 # => 4
h # => {:foo=>4, :bar=>1, :baz=>2, :bat=>3}
```

### Deleting a Hash Value

The simplest way to delete a Hash entry (instance method <tt>delete</tt>):

```ruby
h.delete(:bat) # => 3
h # => {:foo=>4, :bar=>1, :baz=>2}
```

### Default Values

For a Hash key that does not exist, method <tt>[]</tt> returns a default value that is based on both:

* Its default value.
* Its default proc.

#### Default Value

The simplest case is seen when the default proc for the Hash has not been set (i.e., is <tt>nil</tt>). In that case the value of method default (initially <tt>nil</tt>) is returned:

```ruby
h = Hash.new
h.default_proc # => nil
h.default # => nil
h[:nosuch] # => nil
```

You can set the default value with method <tt>default=</tt>:

```ruby
h.default = false
h[:nosuch] # => false
```

For certain kinds of default values (e.g., a String), the default can be modified thus:

h = Hash.new('Foo')
h[:nosuch] # => "Foo"
h[:nosuch_0].upcase! # => "FOO"
h[:nosuch_1] # => "FOO"

#### Default Proc

When the default proc for a Hash is set, the returned default value is determined by the default proc alone
(and the default value is ignored).

For a Hash that's to be created by Hash.new, you can initialize the default proc by including a block:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

You can also set the default proc with method <tt>default_proc=</tt>:

```ruby
h = Hash.new
h.default_proc # => nil
h.default_proc = proc { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

When the default proc is set (i.e., not <tt>nil</tt>) and method <tt>[]</tt> is called with with a non-existent key, the block is called with both the Hash object itself and the missing key; the block's return value is returned as the key's value:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h[:nosuch] # => "Default value for nosuch"
```

In this case, the default value is ignored:

```ruby
h.default = false
h[:nosuch] # => "Default value for nosuch"
```

Note also that in the example above no entry for key <tt>:nosuch</tt> is created:

```ruby
h.include?(:nosuch) # => false
```

However, the block itself can add a new entry:

```ruby
h = Hash.new { |hash, key| hash[key] = "Subsequent value for #{key}"; "First value for #{key}" }
h.include?(:nosuch) # => false
h[:nosuch] # => "First value for nosuch"
h.include?(:nosuch) # => true
h[:nosuch] # => "Subsequent value for nosuch"
h[:nosuch] # => "Subsequent value for nosuch"
```

Setting the default proc to <tt>nil</tt> causes a missing-key reference to return the default value:

```ruby
h.default_proc = nil
h.default = false
h[:nosuch] # => false
```

### Entry Order

A Hash object presents its entries in the of their creation. This is seen in:

* Iterative methods such as <tt>each</tt>, <tt>each_key</tt>, <tt>each_pair</tt>, <tt>each_value</tt>.
* Other order-sensitive methods such as <tt>shift</tt>, <tt>keys</tt>, <tt>values</tt>.
* The String returned by method <tt>inspect</tt>.

A new Hash has its initial ordering per the given entries:

```ruby
h = Hash[foo: 0, bar: 1, baz: 2]
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

#### Hash-Convertible Arguments

Some Hash methods accept one or more Hash-convertible objects as arguments.

Here, "Hash-convertible object" means an object that:
* Has an instance method <tt>to_hash</tt> that.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Hash)</tt> returns <tt>true</tt>.

This class is Hash-convertible:

```ruby
class HashConvertible
  def to_hash
    {foo: 0, bar: 1}
  end
end
h = {}
h.merge(HashConvertible.new) # => {:foo=>0, :bar=>1}
```

Class Integer is not Hash-convertible (no <tt>to_hash</tt> method):

```ruby
h = {}
h.merge(1) # Raises TypeError (no implicit conversion of Integer into Hash)
```

This class is not Hash-convertible (method <tt>to_hash</tt> takes arguments):

```ruby
class NotHashConvertible
  def to_hash(x)
    {foo: 0, bar: 1}
  end
end
h = {}
h.merge(NotHashConvertible.new) # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

This class is not Hash-convertible (method <tt>to_hash</tt> returns non-Hash):

```ruby
class NotHashConvertible
  def to_hash
    :foo
  end
end
h = {}
h.merge(NotHashConvertible.new) # Raises TypeError (can't convert NotHashConvertible to Hash (NotHashConvertible#to_hash gives Symbol))
```

### Public Class Methods

#### ::[]

```ruby
Hash[] → new_empty_hash
Hash[ key, value, ... ] → new_hash
Hash[ [ [ key, value ], ... ] ] → new_hash
Hash[ hashable_object ] → new_hash 
```

Returns a new Hash object populated with the given objects, if any.

The initial default value and default proc are set to nil (see [Default Values](#default-values)):

```Ruby
Hash[].default # => nil
Hash[].default_proc # => nil
```

When an even number of arguments is given, returns a new hash wherein each successive even/odd pair of arguments forms a key/value entry:

```ruby
Hash[] # => {}
Hash[:foo, 0, :bar, 1] # => {:foo=>0, :bar=>1}
```

When the only argument is an array of 2-element arrays, returns a new hash wherein each 2-element array forms a key/value entry:

```ruby
Hash[ [ [:foo, 0], [:bar, 1] ] ] # => {:foo=>0, :bar=>1}
```

When the only argument is an object that is convertible to a hash, converts the object and returns the new hash:
```ruby
class Foo
  def to_hash
    {foo: 0, bar: 1}
  end
end
Hash[Foo.new] # => {:foo=>0, :bar=>1}
```

Note: Here “object that is convertible to a hash” means an object that has has instance method to_hash that takes no arguments and returns a Hash object.

Raises an exception if the argument count is 1, but the argument is not an array of 2-element arrays or an object that is convertible to a hash:

```ruby
Hash[:foo] # Raises ArgumentError (odd number of arguments for Hash)
Hash[ [ [:foo, 0, 1] ] ] # Raises ArgumentError (invalid number of elements (3 for 1..2))
```

Raises an exception if the argument count is odd and greater than 1:

```ruby
Hash[0, 1, 2] # Raises ArgumentError (odd number of arguments for Hash)
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

```ruby
new → new_hash
new(default_value) → new_hash
new { |hash, key| ... } → new_hash
```

Returns a new empty (no entries) Hash object. The new hash has an initial default value and an initial default proc that depend on which form above was used.  See [Default Values](#default-values).

If neither default_value nor block given, initializes both the default value and the default proc to nil

```ruby
h = Hash.new
h.default # => nil
h.default_proc # => nil
h[:nosuch] # => nil
```

If <tt>default_value</tt> given but no block given, initializes the default value to the given value and the default proc to +nil:

```ruby
h = Hash.new(false)
h.default # => false
h.default_proc # => nil
h[:nosuch] # => false
```

If block given but no argument given, stores the block as the default proc, and sets the default value (which will be ignored) to nil:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h.default # => nil
h.default_proc.class # => Proc
h[:nosuch] # => "Default value for nosuch"
```

Raises an exception if both default_value and a block are given:
```ruby
Hash.new(0) { } # Raises ArgumentError (wrong number of arguments (given 1, expected 0))
```


#### ::try_convert

````ruby
 try_convert(obj) → new_hash or nil
````

Returns the new Hash object created by calling
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

Returns nil unless <tt>obj.respond_to?(:to_hash)</tt>:

```ruby
s = 'foo'
s.respond_to?(:to_hash)
Hash.try_convert(s) # => nil
```

Returns nil unless <tt>obj.to_hash</tt> returns a Hash object:

```ruby
class HashableSet < Set
  def to_hash
    nil
  end
end
hs = HashableSet.new([:foo, :bar, :baz])
Hash.try_convert(hs) # => nil
```

### Public Instance Methods

#### <

```ruby
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

Raises an exception if <tt>other_hash</tt> is not a Hash object:

```ruby
h = {}
h < 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### <=

```ruby
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

Raises an exception if <tt>other_hash</tt> is not a Hash object:

```ruby
h = {}
h <= 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```


#### ==

```ruby
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

#### >

```ruby
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

Raises an exception if <tt>other_hash</tt> is not a Hash object:

```ruby
h = {}
h > 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### >=

```ruby
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

Raises an exception if <tt>other_hash</tt> is not a Hash object:

```ruby
h = {}
h >= 1 # Raises TypeError (no implicit conversion of Integer into Hash)
```

#### []

```ruby
hash[key] → value
```

Returns the value object corresponding to key <tt>key</tt>, if found:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h[:foo] # => 0
```

Otherwise, returns the default value (see [Default Values](#default-values)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h[:nosuch] # => default value
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys))):

```ruby
h = {}
h[BasicObject.new] # Raises NoMethodError (undefined method `to_s' for #<BasicObject:>)
```
#### []=

```ruby
hash[key] = value → value
```

Associates <tt>value</tt> with <tt>key</tt>, and returns <tt>value</tt>.

If key <tt>key</tt> exists, replaces its value:

```ruby
h = {foo: 0, bar: 1}
h[:foo] = 2 # => 2
h # => {:foo=>2, :bar=>1}
```

The ordering is not affected; see [Entry Order](#entry-order).

If key <tt>key</tt> does not exist, adds the key and value:

```ruby
h = {foo: 0, bar: 1}
h[:baz] = 2 # => 2
h # => {:foo=>0, :bar=>1, :baz=>2}
```
The new entry is last in the order; see [Entry Order](#entry-order).

Raises an exception if the key is invalid
(see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1}
h[BasicObject.new] = 2 # Raises NoMethodError (undefined method `hash' for #<BasicObject>)
```

#### assoc

```ruby
assoc(key) → new_array or nil
```

If key <tt>key</tt> is found, returns a 2-element Array
containing the key and its value:

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
h.assoc(BasicObject.new) Raises NoMethodError (undefined method `hash' for #<BasicObject>)
```

#### clear

```ruby
clear → self
```

Removes all hash entries, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.clear # => {}
```

#### compact

```ruby
compact → new_hash
```

Returns a copy of <tt>self</tt> with all <tt>nil</tt>-valued entries removed:

```ruby
h = {foo: 0, bar: nil, baz: 2, bat: nil}
h1 = h.compact
h1 # => {:foo=>0, :baz=>2}
```

#### compact!

```ruby
compact → self or nil
```

Returns <tt>self</tt> with all <tt>nil</tt>-valued entries removed:

```ruby
h = {foo: 0, bar: nil, baz: 2, bat: nil}
h.compact!
h # => {:foo=>0, :baz=>2}
```

Returns <tt>nil</tt> if no entries were removed:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.compact! # => nil
h # => {:foo=>0, :bar=>1, :baz=>2}
```

#### compare_by_identiry

```ruby
compare_by_identity → self
```

Sets <tt>self</tt> to consider only identity in comparing keys, returning the hash;
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
h.size # => 1
```

After calling <tt>compare_by_identity</tt>, the keys are considered different,
the therefore do not overwrite:

``ruby
h = {}
h.compare_by_identity # => {}
h.compare_by_identity? # => true
h[s0] = 0
h[s1] = 1
h.size # => 2
``

#### compare_by_identity?

```ruby
compare_by_identity? → true or false
```

Returns <tt>true</tt> if <compare_by_identity> has been called, <tt>false</tt> otherwise:

```ruby
h = {}
h.compare_by_identity? # false
h.compare_by_identity
h.compare_by_identity? # true
```

#### default

```ruby
default → obj
default(key) → obj
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

```ruby
default = obj → obj
```

Sets the default value to <tt>obj</tt>, returning <tt>obj</tt>:

```ruby
h = {}
h.default # => nil
h.default = false # => false
h.default # => false
```

See [Default Values](#default-values).


#### default_proc

```ruby
default_proc → proc or nil
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

```ruby
default_proc = proc → proc
```

Sets the default proc to <tt>proc</tt>:

```ruby
h = {}
h.default_proc # => nil
h.default_proc = proc { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

See [Default Values](#default-values).

Raises an exception if <tt>proc</tt> is not a <tt>Proc</tt> object:

```ruby
h = {}
h.default_proc = 0 # Raises TypeError (wrong default_proc type Integer (expected Proc))
```

#### delete

```ruby
delete(key) → value
delete(key) {| key | ... } → value
```

If no block is given and the hash includes key <tt>key</tt>, deletes its entry and returns the associated value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete(:bar) # => 1
```

If no block given and the hash does not include key <tt>key</tt>, returns <tt>nil</tt>:

```ruby
h.delete(:nosuch) # => nil
```

If a is block given and the hash includes key <tt>key</tt>, ignores the block, deletes the entry,
and returns the associated value:

```ruby
h.delete(:baz) { |key| fail 'Will never happen'} # => 2
```

If a block is given and the hash does not include key <tt>key</tt>,
calls the block and returns the block's return value:

```ruby
h.delete(:nosuch) { |key| "Key #{key} not found" } # => "Key nosuch not found"
```

Raises an exception if <tt>key</tt> is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h.delete(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### delete_if

```ruby
delete_if {| key, value | ... } → new_hash
delete_if → new_enumerator
```

Returns a new Hash object consisting of all entries for which the block returns a truthy value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.delete_if { |key, value| value > 0 } # => {:foo=>0}
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.delete_if # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:delete_if>
e.each { |key, value| value > 0 } # => {:foo=>0}
```

#### dig

```ruby
dig(*keys) → value
```

Returns the value for a specified key in nested objects:

```ruby
h = { foo: {bar: {baz: 2}}}
h.dig(:foo, :bar, :baz) # => 2
```

Returns <tt>nil</tt> if the key is not found:

```ruby
h = { foo: {bar: {baz: 2}}}
h.dig(:foo, :nosuch) # => nil
```

Raises an exception if a traversed entry does not respond to <tt>dig</tt>:

```ruby
h = { foo: [10, 11, 12] }
h.dig(:foo, 1, 0) # Raises TypeError: Integer does not have #dig method
```

Raises an exception if any given key is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h.dig(BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### each

```ruby
each {| key, value | ... } → self
each → new_enumerator
```

Calls the given block with each key-value pair, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each { |key, value| puts "#{key}: #{value}"} # => {:foo=>0, :bar=>1, :baz=>2}
```

Output:

```
foo: 0
bar: 1
baz: 2
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
e = h.each
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each>
e.each { |key, value| puts "#{key}: #{value}"}
```

Output:

```
foo: 0
bar: 1
baz: 2
```

#### each_key

```ruby
each_key { |key| ... } → self
each_key → new_enumerator
```

Calls the given block with each key, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each_key { |key| puts key } # => {:foo=>0, :bar=>1, :baz=>2}
```

Output:

```
foo
bar
baz
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
e = h.each_key
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each+key>
e.each { |key| puts key }
```

Output:

```
foo
bar
baz
```

#### each_pair

```ruby
each_pair {| key, value | ... } → self
each_pair → new_enumerator
```

Calls the given block with each key-value pair, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each_pair { |key, value| puts "#{key}: #{value}"} # => {:foo=>0, :bar=>1, :baz=>2}
```

Output:

```
foo: 0
bar: 1
baz: 2
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
e = h.each
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each+pair>
e.each { |key, value| puts "#{key}: #{value}"}
```

Output:

```
foo: 0
bar: 1
baz: 2
```

#### each_value

```ruby
each_value { |value| ... } → self
each_value → new_enumerator
```

Calls the given block with each value, returning <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.each_value { |value| puts value } # => {:foo=>0, :bar=>1, :baz=>2}
```

Output:

```
0
1
2
```

Returns an <tt>Enumerator</tt> if no block given:

```ruby
e = h.each_value
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each+value>
e.each { |value| puts value }
```

Output:

```
0
1
2
```

#### empty?

```ruby
empty? → true or false
```

Returns <tt>true</tt> if there are no hash entries, <tt>false</tt> otherwise:

```ruby
{}.empty? # => true
{foo: 0, bar: 1, baz: 2}.empty? # => false
```

#### eql?

```ruby
eql? other_hash → true or false
```

Returns <tt>true</tt> if:
* <tt>other_hash</tt> is a Hash object.
* <tt>h</tt> and <tt>other_hash</tt> have the same
  keys (regardless of order).
* For each key _key_, <tt>h[key] eql? other_hash[key]</tt>.

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

```ruby
fetch(key) → value
fetch(key , default) → value
fetch(key) { |key| ... } → value
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
* Otherwise, returns <tt>default</tt>:

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
* If kkey <tt>key</tt> found, returns its associated value.
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

```ruby
fetch_values(*keys) → new_array
fetch_values(*keys) { |key| ... } → new_array
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

```ruby
filter {|key, value| ... } → new_hash
filter → new_enumerator
```

Returns a new Hash object consisting of the entries for which the block returns a truthy value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.filter { |key, value| key.start_with?('b') } # => {:bar=>1, :baz=>2}
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.filter # => {:bar=>1, :baz=>2} # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:filter>
e.each { |key, value| key.start_with?('b') } # => {:bar=>1, :baz=>2}
```

#### filter!

```ruby
filter! {| key, value | ... } → self or nil
filter! → new_enumerator
```

Deletes each hash entry for which the block returns <tt>nil</tt> or <tt>false</tt>, returning the self:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.filter! { |key, value| key.start_with?('b') } # => {:bar=>1, :baz=>2}
```

Returns <tt>nil</tt> if no entries were deleted:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.filter! { |key, value| true } # => nil
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.filter! # => {:bar=>1, :baz=>2} # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:filter!>
e.each { |key, value| key.start_with?('b') } # => {:bar=>1, :baz=>2}
```

#### flatten

```ruby
flatten(level = 1) → new_array 
```

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

Raises an exception if <tt>level</tt> cannot be converted to an Integer:

```ruby
h = {foo: 0, bar: [:bat, 3], baz: 2}
h.flatten(Complex(2, 1)) # Raises RangeError (can't convert 2+1i into Integer)
h.flatten(:nosuch) # Raises TypeError (no implicit conversion of Symbol into Integer)
```

#### has_key?

```ruby
has_key?(key) → true or false
```

Returns <tt>true</tt> if <tt>key</tt> is a key in the hash, otherwise <tt>false</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.has_key?(:bar) # => true
h.has_key?(:nosuch) # => false
h.has_key?(BasicObject.new) # false 
```

#### has_value?

```ruby
 has_value?(value) → true or false
```

Returns <tt>true</tt> if <tt>value</tt> is a value in the hash, otherwise <tt>false</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.has_value?(1) # => true
h.has_value?(123) # => false
```

#### hash

```ruby
hash → an_integer
```

Returns the Integer hash value for the hash:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h1.hash # => 983977779
```

Two Hash objects have the same hash value if their content is the same:

```ruby
h1 = {foo: 0, bar: 1, baz: 2}
h2 = {baz: 2, bar: 1, foo: 0}
h2.hash == h1.hash # => true
h2.eql? h1 # => true
```

#### include?

```ruby
include?(key) → true or false
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

#### inspect

```ruby
inspect → new_string
```

Returns a new String showing the hash entries:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.inspect # => "{:foo=>0, :bar=>1, :baz=>2}"
```

#### invert

```ruby
invert → new_hash
```

Returns a new Hash object with the each key-value pair reversed:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.invert # => {0=>:foo, 1=>:bar, 2=>:baz}
```

Overwrites any repeated new keys:

```ruby
h = {foo: 0, bar: 0, baz: 0}
h.invert # => {0=>:baz}
```

Raises an exception if any value cannot be a key:

```ruby
h = {foo: 0, bar: 1, baz: BasicObject.new}
h.invert NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### keep_if

```ruby
keep_if {| key, value | ... } → self
keep_if → new_enumerator
```

Yields each entry's key-value pair to the block,
retains the entry if the block returns a truthy value,
deletes the entry otherwise,
and returns <tt>self</tt>.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.keep_if { |key, value| key.start_with?('b') } # => {:bar=>1, :baz=>2}
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.keep_if # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:keep_if>
e.each { |key, value| key.start_with?('b') } # => {:bar=>1, :baz=>2}
```

#### key

```ruby
key(value) → key or nil
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

```ruby
key?(key) → true or false
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

#### keys

```ruby
keys → new_array
```

Returns a new Array containing all keys in <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.keys # => [:foo, :bar, :baz]
```

#### length

```ruby
length → an_integer
```

Returns the count the entiries in <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.length # => 3
```

#### member?

```ruby
member?(key) → true or false
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

#### merge

```ruby
merge -> new_copy_of_hash
merge(*other_hashes) → new_hash
merge(*other_hashes) { |key, old_value, new_value| ... } → new_hash 
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
h.merge(h1, h2) # => {:foo=>0, :bar=>4, :baz=>2, :bat=>6, :bam=>5}
```

With arguments and a block:
* Returns a new Hash object that is the merge of <tt>self</tt>
and each given hash.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* For each duplicate key:
  * Yields the key and the old and new values to the block.
  * The block's return value becomes the new value for the entry.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge(h1, h2) { |key, old_value, new_value| old_value + new_value }
h3 # => {:foo=>0, :bar=>5, :baz=>2, :bat=>9, :bam=>5}
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

#### merge!

```ruby
merge! -> self
merge!(*other_hashes) → self
merge!(*other_hashes) { |key, old_value, new_value| ... } → self
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
h3 = h.merge!(h1, h2)
h3 # => {:foo=>0, :bar=>4, :baz=>2, :bat=>6, :bam=>5}
h3.object_id == h.object_id # => true
```

With arguments and a block:
* Returns <tt>self</tt>, after the given hashes are merged.
* The given hashes are merged left to right.
* Each new-key entry is added at the end.
* For each duplicate key:
  * Yields the key and the old and new values to the block.
  * The block's return value becomes the new value for the entry.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h3 = h.merge!(h1, h2) { |key, old_value, new_value| old_value + new_value }
h3 # => {:foo=>0, :bar=>5, :baz=>2, :bat=>9, :bam=>5}
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

#### rassoc

```ruby
rassoc(value) → new_array or nil
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

```ruby
rehash → self
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
h.rehash # => {[:bam, :bar]=>0, [:baz, :bat]=>1}
h.include?(a0) # => true
h[a0] # => 0
```

Raises an exception if called while hash iteration in progress:

```ruby
h = {foo: 0, bar: 1, baz: 1}
h.each { |key, value| h.rehash } # Raises RuntimeError (rehash during iteration)
```

#### reject

```ruby
reject { |key, value| ...} → new_hash
reject → new_enumerator 
```

Returns a new Hash object whose entries are all those from <tt>self</tt>
for which the block returns <tt>false</tt> or <tt>nil</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.reject { |key, value| key.start_with?('b') } # => {:foo=>0}
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.reject
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:reject>
e.each { |key, value| key.start_with?('b') } # => {:foo=>0}
```

#### reject!

```ruby
reject! { |key, value| ... } → self or nil
reject! → new_enumerator
```

Returns <tt>self</tt>, whose remaining entries are all those
for which the block returns <tt>false</tt> or <tt>nil</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.reject! { |key, value| key.start_with?('b') } # => nil
h # => {:foo=>0} # => {:foo=>0, :bar=>1, :baz=>2}
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
e = h.reject!
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:reject!>
e.each { |key, value| key.start_with?('b') } # => {:foo=>0}
```

#### replace

```ruby
replace(other_hash) → self
```

Replaces the entire contents of <tt>self</tt> with the contents of <tt>other_hash</tt>;
returns <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.replace({bat: 3, bam: 4}) # => {:bat=>3, :bam=>4}
```

Raises an exception if <tt>other_hash</tt> is not convertible to a Hash object:

```ruby
h = {}
h.replace(:not_a_hash) # Raises TypeError (no implicit conversion of Symbol into Hash)

```
#### select

```ruby
select { |key, value| ... } → new_hash
select → new_enumerator
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
e = h.select
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:select>
e.each { |key, value| value < 2 } # => {:foo=>0, :bar=>1}
```

#### select!

```ruby
select! { |key, value| ... } → self
select → new_enumerator
```

Returns <tt>self</tt> whose entries are those for which the block returns a truthy value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.select! { |key, value| value < 2 } # => {:foo=>0, :bar=>1}
h # => {:foo=>0, :bar=>1}
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
e = h.select!
e # => # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:select!>
e.each { |key, value| value < 2 } # => {:foo=>0, :bar=>1}
h # => {:foo=>0, :bar=>1}
```

#### shift

```ruby
shift → [key, value] or default_value
```

Removes the first entry from <tt>self</tt>,
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

```ruby
size → an_integer
```

Returns the count of hash entries:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.size # => 3
h.delete(:foo) # => 0
h.size # => 2
```

#### slice

```ruby
slice(*keys) → new_hash
```

Returns a new Hash object containing the entries for the given <tt>*keys</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.slice(:baz, :foo) # => {:baz=>2, :foo=>0}
```

Raises an exception if any given key is invalid (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.slice(:foo, BasicObject.new) # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### store

```ruby
store(key, value) → value
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

```ruby
to_a → new_array
```

Returns a new Array of 2-element Array objects;
each nested Array contains the key and value for a hash entry:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.to_a # => [[:foo, 0], [:bar, 1], [:baz, 2]]
```

#### to_h

```ruby
to_h → self or new_hash
to_h { |key, value| ... } → new_hash
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
h.class # => Hhash = h.to_h
hash # => {:foo=>0, :bar=>1, :baz=>2}
hash.class # => Hash
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

#### to_hash

```ruby
to_hash → self
```

Returns <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.to_hash
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

#### to_proc

```ruby
to_proc → proc
```

Returns a Proc object that maps a key to its value:

```ruby
h = {foo: 0, bar: 1, baz: 2}
proc = h.to_proc
proc.class # => Proc
proc.call(:foo) # => 0
proc.call(:bar) # => 1
proc.call(:nosuch) # => nil
[:foo, :bar, :noosuch].map(&h) # => [0, 1, nil]
```

#### to_s

```ruby
to_s → new_string
```

Returns a new String showing the hash entries:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.to_s # => "{:foo=>0, :bar=>1, :baz=>2}"
```

#### transform_keys

```ruby
transform_keys {|key| ... } → new_hash
transform_keys → new_enumerator
```

Returns a new Hash object of size <tt>self.size</tt>;
each entry has:'
* A key provided by the block.
* The value from <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_keys { |key| key.to_s } # => {"foo"=>0, "bar"=>1, "baz"=>2}
```

Returns a new Enumerator if no block given:
```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_keys
e.each { |key| key.to_s } # => {"foo"=>0, "bar"=>1, "baz"=>2}
```

Raises an exception if the block returns an invalid key (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_keys { |key| BasicObject.new } # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### transform_keys!

```ruby
transform_keys! {|key| ... } → self
transform_keys! → new_enumerator
```

Returns <tt>self</tt> with new keys provided by the block:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = h.transform_keys! { |key| key.to_s }
h1 # => {"foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => true
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_keys!
h1 = e.each { |key| key.to_s }
h1 # => {"foo"=>0, "bar"=>1, "baz"=>2}
h1.object_id == h.object_id # => true
```

Raises an exception if the block returns an invalid key (see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_keys! { |key| BasicObject.new } # Raises NoMethodError (undefined method `hash' for #<BasicObject:>)
```

#### transform_values

```ruby
transform_values {|value| ... } → new_hash
transform_values → new_enumerator
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
e = h.transform_values
e
h1 = e.each { |value| value * 100}
h1 # => {:foo=>0, :bar=>100, :baz=>200}
h1.object_id == h.object_id # => false
```

#### transform_values!

```ruby
transform_values {|value| ... } → self
transform_values → new_enumerator
```

Returns <tt>self</tt>, whose keys are unchanged,
and whose values are determined by the given block.

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.transform_values! { |value| value * 100} # => {:foo=>0, :bar=>100, :baz=>200}
h # => {:foo=>0, :bar=>100, :baz=>200}
```

Returns a new Enumerator if no block given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.transform_values!
e.each { |value| value * 100} # => {:foo=>0, :bar=>100, :baz=>200}
h # => {:foo=>0, :bar=>100, :baz=>200}
```

#### update

```ruby
update -> self
update(*other_hashes) → self
update(*other_hashes) { |key, old_value, new_value| ... } → self
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
  * Yields the key and the old and new values to the block.
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

Raises an exception if any given argument is not convertible to a Hash object:

```ruby
h = {}
h.merge(:foo) # Raises TypeError (no implicit conversion of Symbol into Hash)
```
#### value

```ruby
value?(value) → true or false
```

Returns <tt>true</tt> if <tt>value</tt> is a value in the hash,
<tt>false</tt> otherwise:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.value?(0) # => true
h.value?(3) # => false
```

#### values

```ruby
values → new_array
```

Returns a new Array containing all values in <tt>self</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values # => [0, 1, 2]
```

#### values_at

```ruby
values_at(*keys) → new_array
```

Returns a new Array containing values for the given <tt>*keys</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values_at(:foo, :baz) # => [0, 2]
```

Returns an empty Array if no arguments given:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values_at # => [0, 2]
```

Raises an exception if any given key is invalid
(see [Invalid Hash Keys](#invalid-hash-keys)):

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.values_at(BasicObject.new) # => [0, 2]
```
