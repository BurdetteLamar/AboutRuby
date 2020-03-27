## About Hash

A Hash is a dictionary-like collection of key-value pairs,
wherein the keys are unique. (The values need not be unique.)

A Hash has certain similarities to an Array, but:
* An Array index is always an Integer.
* A Hash key can be (almost) any object.

### Contents
- [Hash Data Syntax](#hash-data-syntax)
- [Common Uses](#common-uses)
- [Creating a Hash](#creating-a-hash)
  - [Constructor Hash.new](#constructor-hashnew)
  - [Hash Literal](#hash-literal)
  - [Hash Implicit Form](#hash-implicit-form)
- [Hash Value Basics](#hash-value-basics)
  - [Getting a Hash Value](#getting-a-hash-value)
  - [Setting a Hash Value](#setting-a-hash-value)
  - [Deleting a Hash Value](#deleting-a-hash-value)
- [Chaining Method Calls](#chaining-method-calls)

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

You can use a Hash to give names to objects:

```ruby
matz = {name: 'Matz', language: 'Ruby'}
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
matz = Dev.new(name: 'Matz', language: 'Ruby')
matz # => #<Dev: @name="Matz", @language="Ruby">
```

Note: when the last argument in a method call is a Hash,
the curly braces may be omitted:

```ruby
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
