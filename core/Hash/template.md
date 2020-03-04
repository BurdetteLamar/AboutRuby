## Hash

A Hash is a dictionary-like collection of unique keys and their corresponding values.

A Hash has certain similarities to an Array, but while an Array index is always an Integer, a Hash key can be any object.

@[:page_toc](### Contents)

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

If all initial keys are to be Symbols, there's this shorthand:

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
h = {:foo => 0, :bar => 1, :baz => 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

If all initial keys are to be Symbols, there's this shorthand:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

### Getting a Hash Value

The simplest way to get a Hash value (instance method []):

```ruby
h[:foo] # => 0
```

### Setting a Hash Value

The simplest way to create or update a Hash value (instance method []=):

```ruby`
h[:bat] = 3 # => 3
h # => {:foo=>0, :bar=>1, :baz=>2, :bat=>3}
h[:foo] = 4 # => 4
h # => {:foo=>4, :bar=>1, :baz=>2, :bat=>3}
```

### Deleting a Hash Value

The simplest way to delete a Hash entry (instance method delete):

```ruby
h.delete(:bat) # => 3
h # => {:foo=>4, :bar=>1, :baz=>2}
```

### Default Values

For a Hash key that does not exist, method [] returns a default value that is based on both:

* Its default value.
* Its default proc.

#### Default Value

The simplest case is seen when the default proc for the Hash has not been set (i.e., is nil). In that case the value of method default (initially nil) is returned:

```ruby
h = Hash.new
h.default_proc # => nil
h.default # => nil
h[:nosuch] # => nil
```

Set the default value with method default=:

```ruby
h.default = false
h[:nosuch] # => false
```

#### Default Proc

When the default proc for a Hash is set, the returned default value is determined by the default proc alone.

For a Hash that's to be created by Hash.new, you can initialize the default proc by including a block:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

You can set the default proc with method default_proc=:

```ruby
h = Hash.new
h.default_proc # => nil
h.default_proc = proc { |hash, key| "Default value for #{key}" }
h.default_proc.class # => Proc
```

When the default proc is set (i.e., not nil) and method [] is called with with a non-existent key, the block is called with both the Hash object itself and the missing key, and the block's return value is returned as the key's value:

```ruby
h = Hash.new { |hash, key| "Default value for #{key}" }
h[:nosuch] # => "Default value for nosuch"
```

Note that in this case, the default value is ignored:

```ruby
h.default = false
h[:nosuch] # => "Default value for nosuch"
```

Note also that in the example above no entry for key :nosuch is created:

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

Delete the entry for key :nosuch (because we use it in these examples as a missing key):

```ruby
h.delete(:nosuch)
```

Setting the default proc to nil causes a missing-key reference to return the default value:

```ruby
h.default_proc = nil
h.default = false
h[:nosuch] # => false
```

### Entry Order

A Hash object presents its entries in the or their creation. This is seen in:

* Iterative methods such as each, each_key, each_pair, each_value.
* Other order-sensitive methods such as shift, keys, values.
* The String returned by method inspect.

A new Hash:

```ruby
h = Hash[:foo => 0, :bar => 1, :baz => 2]
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

I can't improve on the discussion over at [ruby-doc.org](https://ruby-doc.org/core-2.7.0/Hash.html#class-Hash-label-Hash+Keys) (and don't want to steal from it).

Except to add an example:

```ruby
{BasicObject.new => 0} # Raises NoMethodError (undefined method `hash' for #<BasicObject)
```