## ENV

### What's ENV?

From the documentation for Ruby <code>ENV</code>:

>ENV is a hash-like accessor for environment variables.

### Interaction with the Operating System

The <code>ENV</code> object communicates
with the operating system's environment variables.

When you get the value for a name in <code>ENV</code>,
the value is retrieved from among the current environment variables.

When you create or set a name-value pair in <code>ENV</code>,
the name and value are immediately set in the environment variables.

When you delete a name-value pair in ```ENV```,
it is immediately deleted from the environment variables.

### Names and Values

Generally speaking, each name or value is a ```String```.

@[:markdown](restrictions.md)

### About Ordering

The ordering of ```ENV``` content is OS-dependent.

This will be seen in:
- A ```Hash``` returned by an ```ENV``` method.
- An ```Enumerator``` returned by an ```ENV``` method.
- An ```ENV``` method that iterates over its names, values, or name-value pairs..
- The ```String``` returned by ```ENV#shift```.
- The ```String``` returned by ```ENV#inspect```.

@[:markdown](../../include_files/begin_irb.md)

### About the Examples

Some methods in ```ENV``` return ```ENV``` itself.
Typically, there are many environment variables.
It's not useful to display a large ```ENV``` in the examples here,
so let's begin with it empty:

```#run_irb
ENV.clear
p ENV.size
```

Also, each example "inherits" the state of ```ENV``` from those preceding it.

@[:page_toc](### Contents)

### The Basics

#### Setting an Environment Variable

Set environment variable <code>foo</code>:

```#run_irb
p ENV['foo'] = '0'
```

#### Getting an Environment Variable

Get environment variable <code>foo</code>:

```#run_irb
p ENV['foo']
```

#### Deleting an Environment Variable

Delete environment variable <code>foo</code>:

```#run_irb
p ENV['foo'] = nil
```

### Setter Methods

#### ENV#[]=

```ruby
ENV[name] = value # => value
```

Use <code>ENV#[]=</code> to create, update, or delete an environment variable.
The method returns the environment variable's value.

Create an environment variable:

```#run_irb
p ENV['foo'] = '0'
p ENV['foo']
```

Update an environment variable:

```#run_irb
p ENV['foo'] = '1'
p ENV['foo']
```

Delete an environment variable by setting its value to ```nil```:

```#run_irb
p ENV['foo'] = nil
p ENV['foo']
```

Delete a non-existent environment variable (raises no exception):

```#run_irb
p ENV['foo'] = nil
```

Give a name that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV[Object.new] = '0'
rescue => x
  p x
end
```

Give a value that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV['foo'] = Object.new
rescue => x
  p x
end
```

Give a string name that's not allowed (raises ```Errno::EINVAL```):

```#run_irb
begin
  ENV['foo='] = '0'
rescue => x
  p x
end
```

#### ENV#store

```ruby
ENV.store(name, value) # => value
```

Method <code>ENV#store</code> is an alias for method <code>ENV#[]=</code>.

Use <code>ENV#store</code> to create, update, or delete an environment variable.
The method returns the environment variable's value.

Create an environment variable:

```#run_irb
ENV.delete('foo')
p ENV.store('foo', '0')
p ENV['foo']
```

Update an environment variable:

```#run_irb
p ENV.store('foo', '1')
p ENV['foo']
```

Delete an environment variable by setting its value to ```nil```:

```#run_irb
p ENV.store('foo', nil)
p ENV['foo']
```

Delete a non-existent environment variable (raises no exception):

```#run_irb
p ENV.store('foo', nil)
```

Give a name that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV.store(Object.new, '0')
rescue => x
  p x
end
```

Give a value that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV.store('foo', Object.new)
rescue => x
  p x
end
```

Give a string name that's not allowed (raises ```Errno::EINVAL```):

```#run_irb
begin
  ENV.store('foo=',  '0')
rescue => x
  p x
end
```

#### ENV#delete

```ruby
ENV.delete(name) # => value
```

Use method <code>ENV#delete</code> to delete an environment variable.

The method returns the value of the deleted environment variable.

Delete an existing environment variable:

```#run_irb
ENV['foo'] = '0'
p ENV.delete('foo')
p ENV['foo']
```

"Delete" a non-existent environment variable:

```#run_irb
p   ENV.delete('foo')
p ENV['foo']
```

Give a name that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV.delete(Object.new)
rescue => x
  p x
end
```

#### ENV#update

```ruby
ENV.update(hash) # => ENV
```

Use method <code>ENV#update</code> to create, update, and delete
multiple environment variables, all at once.

The method accepts a ```Hash``` argument of name-value pairs,
and returns <code>ENV</code>.

Create environment variables:

```#run_irb
ENV.update('foo' => '0', 'bar' => '1')
p ENV
```

Update environment variables:

```#run_irb
ENV.update('foo' => '2', 'bar' => '3')
p ENV
```

Delete environment variables:

```#run_irb
ENV.update('foo' => nil, 'bar' => nil)
p ENV
```

Do all three at once;

```#run_irb
ENV.update('bar' => '1', 'baz' => '2')
p ENV
ENV.update('foo' => '0', 'bar' => '2', 'baz' => nil)
p ENV
```

Give a name that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV.update(Object.new => '0')
rescue => x
  p x
end
p ENV
```

Give a value that's not a ```String``` (raises ```TypeError```):

```#run_irb
begin
  ENV.update('foo' => Object.new)
rescue => x
  p x
end
p ENV
```

The pairs in the given ```Hash``` are processed in the order given.
Processing continues to the last pair, or until an error occurs.
Whan an error occurs, the processing stops, and no further changes are made.

Give a non-first name that's not a ```String``` (raises ```TypeError``` and processing stops):

```#run_irb
begin
  ENV.update('foo' => '1', Object.new => '0', 'baz' => '2')
rescue => x
  p x
end
p ENV
```

Give a non-first value that's not a ```String``` (raises ```TypeError``` and processing stops):

```#run_irb
begin
  ENV.update('foo' => '0', 'bar' => Object.new, 'baz' => '2')
rescue => x
  p x
end
p ENV
```

#### ENV#replace

```ruby
ENV.replace(hash) # => ENV
```

Use method <code>ENV#replace</code> to replace all environment variables with new ones.

The method accepts a ```Hash``` argument of name-value pairs,
and returns <code>ENV</code>.

Replace ```ENV``` with data from a ```Hash```:

```#run_irb
p ENV
ENV.replace('baz' => '0', 'bat' => '1')
p ENV
```
Give an argument that's not a ```Hash```:

```#run_irb
ENV.replace(Object.new)
p ENV
```

Give a ```Hash``` with an illegal name:

```#run_irb
ENV.replace(Object.new => '0')
p ENV
```

Give a ```Hash``` with an illegal value:

```#run_irb
ENV.replace('foo' => Object.new)
p ENV
```

#### ENV#clear

```ruby
ENV.clear # => ENV
```

Use method ```ENV#clear``` to remove all environment variables.

The method returns ```ENV```.

```#run_irb
saved_env = ENV.to_hash
p ENV.clear
ENV.replace(saved_env)
```

#### ENV#shift

```ruby
ENV.shift # => [name, value]
```

Use method ```ENV#shift``` to remove and return the first environment variable.

The method returns a 2-element ```Array``` of the removed name and value.

```#run_irb
p ENV
p ENV.shift
p ENV
```

### Getter Methods

#### ENV#[]
#### ENV#fetch
#### ENV#key
#### ENV#keys
#### ENV#assoc
#### ENV#values
#### ENV#values_at
#### ENV#invert
#### ENV#rassoc
#### ENV#slice
#### ENV#to_a
#### ENV#to_h
#### ENV#to_hash
#### ENV#to_s
#### ENV#inspect

### Enumeration Methods

#### ENV#delete_if
#### ENV#each
#### ENV#each_key
#### ENV#each_pair
#### ENV#each_value
#### ENV#filter
#### ENV#filter!
#### ENV#keep_if
#### ENV#reject
#### ENV#reject!
#### ENV#select
#### ENV#select!

### Block-Oriented Methods

#### ENV#delete_if
#### ENV#each
#### ENV#each_key
#### ENV#each_pair
#### ENV#each_value
#### ENV#fetch
#### ENV#filter
#### ENV#filter!
#### ENV#keep_if
#### ENV#reject
#### ENV#reject!
#### ENV#select
#### ENV#select!

### Query Methods

#### ENV#empty?
#### ENV#has_key?
#### ENV#has_value?
#### ENV#include?
#### ENV#length
#### ENV#key?
#### ENV#member?
#### ENV#size
#### ENV#value?

### More

- [Documentation](https://ruby-doc.org/core-2.6.4/Exception.html)
- [Source code](https://github.com/ruby/ruby/blob/master/hash.c)
- [Related gems](https://rubygems.org/search?query=env)
- [Best practices](https://www.google.com/search?q=ruby+env+best+practice)
- [Performance](https://www.google.com/search?q=ruby+senv+performance)
