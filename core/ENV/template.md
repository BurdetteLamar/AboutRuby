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

### Restriction

In <code>ENV</code> each name is a <code>String</code>,
as is each value.

@[:markdown](../../include_files/begin_irb.md)

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

### Setter Methods

#### Method #[]=

```ruby
ENV[name] = value
```

@[:markdown](name_details.md)

Use <code>ENV#[]=</code> to create, update, or delete an environment variable.
The method returns the environment variable's value.

Create an environment variable:

```#run_irb
ENV.delete('foo')
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

Give a name that's not a non-allowed ```String``` (raises ```Errno::EINVAL```):

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

Give a name that's not a non-allowed ```String``` (raises ```Errno::EINVAL```):

```#run_irb
begin
  ENV.store('foo=',  '0')
rescue => x
  p x
end
```

#### Method #update

Use method <code>ENV#update</code> to create, update, and delete
multiple environement variables, all at once.

The method accepts a ```Hash``` argument of name-value pairs,
and returns <code>ENV</code>.

Create environment variables:

```run_irb
ENV.update('foo' =< '1')
```

#### Method #replace
#### Method #delete
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
