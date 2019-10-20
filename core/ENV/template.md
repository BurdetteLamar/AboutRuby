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

@[:markdown](../../include_files/begin_irb.md)

### About the Examples

Some of the methods in ```ENV``` return ```ENV``` itself.
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

### Setting an Environment Variable

Set environment variable <code>foo</code>:

```#run_irb
p ENV['foo'] = '0'
```

### Getting an Environment Variable

Get environment variable <code>foo</code>:

```#run_irb
p ENV['foo']
```

### Deleting an Environment Variable

Delete environment variable <code>foo</code>:

```#run_irb
p ENV['foo'] = nil
```

### Setter Methods

#### Method #[]=

```ruby
ENV[name] = value
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

#### Method #store

```ruby
ENV.store(name, value)
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

#### Method #delete

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

#### Method #update

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
p ENv
ENV.update('foo' => '0', 'bar' => '2', 'baz' => nil)
p ENV
```

Give a name that's not a ```String``` (raises ```TypeError``` and no changes are made):

```#run_irb
begin
  ENV.update('foo' => '1', Object.new => '2')
rescue => x
  p x
end
p ENV
```

Give a value that's not a ```String``` (raises ```TypeError``` and no changes are made):

```#run_irb
begin
  ENV.update('foo' => '1', 'bar' => Object.new)
rescue => x
  p x
end
p ENV
```

#### Method #replace

Use method <code>ENV#replace</code> to replace all environment variables with new ones.

The method accepts a ```Hash``` argument of name-value pairs,
and returns <code>ENV</code>.



#### Method #clear
#### Method #shift

### Getter Methods

#### Method #[]
#### Method #fetch
#### Method #key
#### Method #keys
#### Method #assoc
#### Method #values
#### Method #values_at
#### Method #invert
#### Method #rassoc
#### Method #slice
#### Method #to_a
#### Method #to_h
#### Method #to_hash
#### Method #to_s
#### Method #inspect

### Enumeration Methods

#### Method #delete_if
#### Method #each
#### Method #each_key
#### Method #each_pair
#### Method #each_value
#### Method #filter
#### Method #filter!
#### Method #keep_if
#### Method #reject
#### Method #reject!
#### Method #select
#### Method #select!

### Block-Oriented Methods

#### Method #delete_if
#### Method #each
#### Method #each_key
#### Method #each_pair
#### Method #each_value
#### Method #fetch
#### Method #filter
#### Method #filter!
#### Method #keep_if
#### Method #reject
#### Method #reject!
#### Method #select
#### Method #select!

### Query Methods

#### Method #empty?
#### Method #has_key?
#### Method #has_value?
#### Method #include?
#### Method #length
#### Method #key?
#### Method #member?
#### Method #size
#### Method #value?

### More

- [Documentation](https://ruby-doc.org/core-2.6.4/Exception.html)
- [Source code](https://github.com/ruby/ruby/blob/master/hash.c)
- [Related gems](https://rubygems.org/search?query=env)
- [Best practices](https://www.google.com/search?q=ruby+env+best+practice)
- [Performance](https://www.google.com/search?q=ruby+senv+performance)
