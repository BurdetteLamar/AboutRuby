## About Hash

A Hash is a dictionary-like collection of key-value pairs,
wherein the keys are unique. (The values need not be unique.)

A Hash has certain similarities to an Array, but:
* An Array index is always an Integer.
* A Hash key can be (almost) any object.

@[:page_toc](### Contents)

@[:markdown](../include/syntax.md)

### Common Uses

#### Giving Names to Objects

You can use a Hash to give names to objects:

```ruby
matz = {name: 'Matz', language: 'Ruby'}
matz # => {:name=>"Matz", :language=>"Ruby"}
```

#### Giving Names to Arguments

You can use a Hash to give names to method arguments:

```ruby
def some_method(hash)
  p hash
end
some_method({foo: 0, bar: 1, baz: 2}) # => {:foo=>0, :bar=>1, :baz=>2}
```

Note: when the last argument in a method call is a Hash,
the curly braces may be omitted:

```ruby
some_method(foo: 0, bar: 1, baz: 2) # => {:foo=>0, :bar=>1, :baz=>2}
```
#### Initializing Objects

You can use a Hash to initialize an object:

```ruby
class Dev
  attr_accessor :name, :language
  def initialize(hash)
    hash.each_pair do |key, value|
      setter_method = "#{key}=".to_sym
      send(setter_method, value)
    end
  end
end
matz = Dev.new(name: 'Matz', language: 'Ruby')
matz # => #<Dev: @name="Matz", @language="Ruby">
```

### Creating a Hash

Here are three ways to create a Hash:
* Constructor method: <tt>Hash.new.</tt>
* Literal method: <tt>Hash[]</tt>.
* Implicit Form: <tt>{}</tt>.

#### Constructor Hash.new

You can create a Hash by using the constructor method, <tt>Hash.new</tt>.

Create an empty Hash:

```ruby
h = Hash.new
h # => {}
h.class # => Hash
```

#### Hash Literal

You can create a Hash by using its literal method, <tt>Hash[]</tt>.

Create an empty Hash:

```ruby
h = Hash[]
h # => {}
```

Create a Hash with initial entries:

```ruby
h = Hash[foo: 0, bar: 1, baz: 2]
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
h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

### Hash Value Basics

#### Getting a Hash Value

The simplest way to get a Hash value (instance method <tt>[]</tt>):

```ruby
h[:foo] # => 0
```

#### Setting a Hash Value

The simplest way to create or update a Hash value (instance method <tt>[]=</tt>):

```ruby
h[:bat] = 3 # => 3
h # => {:foo=>0, :bar=>1, :baz=>2, :bat=>3}
h[:foo] = 4 # => 4
h # => {:foo=>4, :bar=>1, :baz=>2, :bat=>3}
```

#### Deleting a Hash Value

The simplest way to delete a Hash entry (instance method <tt>delete</tt>):

```ruby
h.delete(:bat) # => 3
h # => {:foo=>4, :bar=>1, :baz=>2}
```

### Chaining Method Calls

Many Hash instance methods return <tt>self</tt>.
For those methods, you can chain method calls:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h.keep_if { |key, value| key.start_with?('b') }.size # => 2
```

### Method Calls with Blocks

For some Hash methods, a call to the method can include a block:
* [Hash.new { |hash, key| ... } → new_hash](../api/markdown.md#new) <br>
  The block initializes the default proc.
  
There are several other families of methods that can call blocks:
* [General iterators](#general-iterators):
  Some Hash methods iterate over hash entries,
  doing only what the block specifies.
* [Specialized iterators](#specialized-iterators):
  Some Hash methods iterate over hash entries,
  performing more specific processing.
* [Missing-key handlers](#missing-key-handlers):
  Some Hash methods can handle missing keys in a block.
* [Duplicate-key handlers](#duplicate-key-handlers):
  Some Hash methods can handle duplicate keys in a block.
  

#### General Iterators

Each of these general iterators traverses Hash entries,
doing only what the block specifies,
and returning <tt>self</tt>:
* [hash.each_pair { |key, value| ... } → self
](../api/markdown.md#each_pair)<br>
  Passes each key-value pair to the block.
* [hash.each { |key, value| ... } → self
](../api/markdown.md#each)<br>
  Alias for <tt>hash.each</tt>.
* [hash.each_key { |key| ... } → self
](../api/markdown.md#each_key)<br>
  Passes each key pair to the block.
* [hash.each_value { |value| ... } → self
](../api/markdown.md#each_value)<br>
  Passes each value pair to the block.

#### Specialized Iterators

* [hash.to_h { |key, value| ... } → new_hash
](../api/markdown.md#to_h)<br>
  Passes each key-value pair to the block;
  returns a new Hash based on the block's return values.

Each of these iterators returns a new Hash,
including or excluding entries
based on whether the block returns a truthy value:
* [hash.filter { |key, value| ... } → new_hash
](../api/markdown.md#filter)<br>
  Passes each key-value pair to the block;
  includes the entry if and only if
  the block returns a truthy value.
* [hash.select { |key, value| ... } → new_hash
](../api/markdown.md#select)<br>
  Alias for <tt>hash.filter</tt>.
* [hash.reject { |key, value| ...} → new_hash
](../api/markdown.md#reject)<br>
  Passes each key-value pair to the block;
  includes the entry if and only if
  the block returns a <tt>false</tt> or <tt>nil</tt>.

Each of these iterators returns <tt>self</tt>,
retaining or deleting entries
based on whether the block returns a truthy value:
* [hash.delete_if { |key, value| ... } → self
](../api/markdown.md#delete_if)<br>
  Passes each key-value pair to the block;
  deletes each entry for which the block returns
  a truthy value.
* [hash.keep_if { |key, value| ... } → self
](../api/markdown.md#keep_if)<br>
  Passes each key-value pair to the block;
  deletes each entry for which the block returns
  <tt>false</tt> or <tt>nil</tt>.
* [ash.filter! { |key, value| ... } → self or nil
](../api/markdown.md#filter-1)<br>
  Passes each key-value pair to the block;
  deletes each entry for which the block returns
  <tt>false</tt> or <tt>nil</tt>.
  (Differs from <tt>hash.keep_if</tt> by returning
  <tt>nil</tt> if no change.)
* [hash.select! { |key, value| ... } → self or nil
](../api/markdown.md#select-1)<br>
  Alias for <tt>hash.filter!</tt>
* [hash.reject! { |key, value| ... } → self or nil
](../api/markdown.md#reject-1)<br>
  Passes each key-value pair to the block;
  deletes each entry for which the block returns
  a truthy value.
  (Differs from <tt>hash.delete_if</tt> by returning
  <tt>nil</tt> if no change.)

Each of these iterators returns a new Hash
with modified keys or values based on the block:
* [hash.transform_keys { |key| ... } → new_hash
](../api/markdown.md#transform_keys)<br>
  Passes each key to the block,
  whose return value becomes a key in the new Hash.
* [hash.transform_values { |value| ... } → new_hash
](../api/markdown.md#transform_values)<br>
  Passes each value to the block,
  whose return value becomes a value in the new Hash.

Each of these iterators returns <tt>self</tt>,
modifying keys or values based on the block:
* [hash.transform_keys! { |key| ... } → self
](../api/markdown.md#transform_keys-1)<br>
  Passes each key to the block,
  whose return value replaces the key.
* [hash.transform_values! { |value| ... } → self
](../api/markdown.md#transform_values-1)<br>
  Passes each value to the block,
  whose return value replaces the value.

#### Missing-Key Handlers

Each of these methods can handle missing keys in a block:
* [hash.delete(key) { |key| ... } → value
](../api/markdown.md#delete)<br>
  Calls the block if <tt>key</tt> is not found;
  returns the block's return value.
* [hash.fetch(key) { |key| ... } → value
](../api/markdown.md#fetch)<br>
  Calls the block if <tt>key</tt> is not found;
  returns the block's return value.
* [hash.fetch_values(*keys) { |key| ... } → new_array](../api/markdown.md#fetch_values)<br>
  Calls the block with each missing key;
  treats the block's return value as the value for that key.

#### Duplicate-Key Handlers
Each of these methods can handle duplicate keys in a block:
* [hash.merge(*other_hashes) { |key, old_value, new_value| ... } → new_hash
](../api/markdown.md#merge)<br>
  Returns a new Hash that is the merge of <tt>self</tt>
  and <tt>*other_hashes</tt>.
  For each duplicate key, calls the block with the key
  and both values; the block's return value becomes the new value.
* [hash.merge!(*other_hashes) { |key, old_value, new_value| ... } → self
](../api/markdown.md#merge-1)<br>
  Returns <tt>self</tt> with <tt>*other_hashes</tt> merged.
  For each duplicate key, calls the block with the key
  and both values; the block's return value becomes the new value.
* [hash.update(*other_hashes) { |key, old_value, new_value| ... } → self
](../api/markdown.md#update)<br>
  Alias for <tt>hash.merge!</tt>.

### Methods Returning Enumerators

Each Hash method that allows a block for iteration
returns an Enumerator if no block is given.
These are:
* [hash.delete_if](../api/markdown.md#delete_if)
* [hash.each](../api/markdown.md#each)
* [hash.each_key](../api/markdown.md#each_key)
* [hash.each_pair](../api/markdown.md#each_pair)
* [hash.each_value](../api/markdown.md#each_value)
* [hash.filter](../api/markdown.md#filter)
* [hash.filter!](../api/markdown.md#filter-1)
* [hash.keep_if](../api/markdown.md#keep_if)
* [hash.reject](../api/markdown.md#reject)
* [hash.reject!](../api/markdown.md#reject-1)
* [hash.select](../api/markdown.md#select)
* [hash.select!](../api/markdown.md#select-1)
* [hash.transform_keys](../api/markdown.md#transform_keys)
* [hash.transform_keys!](../api/markdown.md#transform_keys-1)
* [hash.transform_values](../api/markdown.md#transform_values)
* [hash.transform_values!](../api/markdown.md#transform_values-1)

Example using method <tt>hash.each_pair</tt>:

```ruby
h = {foo: 0, bar: 1, baz: 2}
e = h.each_pair
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each_pair>`
```

And perhaps later on:
```ruby
h1 = e.each { |key, value| puts "#{key}: #{value}"}
h1 # => {:foo=>0, :bar=>1, :baz=>2}
h1.object_id == h.object_id # => true
```

The enumerator has a _reference to_ the Hash, not a copy of it:

```ruby
e # => #<Enumerator: {:foo=>0, :bar=>1, :baz=>2}:each_pair>`
h.delete(:baz) # => 2
e # => #<Enumerator: {:foo=>0, :bar=>1}:each_pair>
```

But making the original Hash unavailable causes the enumerator to make a copy:

```ruby
h = nil
e # => #<Enumerator: {:foo=>0, :bar=>1}:each_pair>
```

