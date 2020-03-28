## About Hash

A Hash is a dictionary-like collection of key-value pairs,
wherein the keys are unique. (The values need not be unique.)

A Hash has certain similarities to an Array, but:
* An Array index is always an Integer.
* A Hash key can be (almost) any object.

### Contents
- [Hash Data Syntax](#hash-data-syntax)
- [Common Uses](#common-uses)
  - [Giving Names to Objects](#giving-names-to-objects)
  - [Giving Names to Arguments](#giving-names-to-arguments)
  - [Initializing Objects](#initializing-objects)
- [Creating a Hash](#creating-a-hash)
  - [Constructor Hash.new](#constructor-hashnew)
  - [Hash Literal](#hash-literal)
  - [Hash Implicit Form](#hash-implicit-form)
- [Hash Value Basics](#hash-value-basics)
  - [Getting a Hash Value](#getting-a-hash-value)
  - [Setting a Hash Value](#setting-a-hash-value)
  - [Deleting a Hash Value](#deleting-a-hash-value)
- [Chaining Method Calls](#chaining-method-calls)
- [Method Calls with Blocks](#method-calls-with-blocks)
  - [General Iterators](#general-iterators)
  - [Specialized Iterators](#specialized-iterators)
  - [Missing-Key Handlers](#missing-key-handlers)
  - [Duplicate-Key Handlers](#duplicate-key-handlers)

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
* [Hash.new](../api/markdown.md#new) with a block initializes the default proc.
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
doing only what the block specifies:
* [hash#each_pair](../api/markdown.md#each)
  passes each key-value pair to the block.
* [hash#each](../api/markdown.md#each) is an alias
  for <tt>hash#each_pair</tt>.
* [hash#each_key](../api/markdown.md#each)
  passes each key pair to the block.
* [hash#each_value](../api/markdown.md#each)
  passes each value pair to the block.

#### Specialized Iterators

[hash#to_h](../api/markdown.md#to_h)
passes each key-value pair to the block
and returns a new Hash based on the block's return values.

Each of these iterators returns <tt>self</tt>,
retaining or deleting entries
based on whether the block returns a truthy value:
* [hash#delete_if](../api/markdown.md#delete_if)
  passes each key-value pair to the block
  and deletes each entry for which the block returns
 a truthy value.
* [hash#keep_if](../api/markdown.md#delete_if)
  does the opposite:
  it passes each key-value pair to the block
  and deletes each entry for which the block returns
  <tt>false</tt> or <tt>nil</tt>.
hash.filter! { |key, value| ... } → self or nil
hash.select! { |key, value| ... } → self
hash.reject! { |key, value| ... } → self or nil

Each of these iterators returns a new Hash,
including or excludng entries
based on whether the block returns a truthy value:
hash.filter { |key, value| ... } → new_hash
hash.select { |key, value| ... } → new_hash
hash.reject { |key, value| ...} → new_hash

Each of these iterators returns <tt>self</tt>,
modifying keys or values based on the block:
hash.transform_keys! { |key| ... } → self
hash.transform_values! { |value| ... } → self

Each of these iterators returns a new Hash
with modified keys or values based on the block:
hash.transform_keys { |key| ... } → new_hash
hash.transform_values { |value| ... } → new_hash

#### Missing-Key Handlers

Each of these methods can handle missing keys in a block:
hash.delete(key) { |key| ... } → value
hash.fetch(key) { |key| ... } → value
hash.fetch_values(*keys) { |key| ... } → new_array

#### Duplicate-Key Handlers
Each of these methods can handle duplicate keys in a block:
hash.merge(*other_hashes) { |key, old_value, new_value| ... } → new_hash
hash.merge!(*other_hashes) { |key, old_value, new_value| ... } → self
hash.update(*other_hashes) { |key, old_value, new_value| ... } → self

