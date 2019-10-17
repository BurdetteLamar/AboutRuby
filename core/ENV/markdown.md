<!-- >>>>>> BEGIN GENERATED FILE (include): SOURCE core/ENV/template.md -->
<!-- >>>>>> BEGIN INCLUDED FILE (markdown): SOURCE include_files/begin_irb.md -->
<!-- >>>>>> BEGIN INCLUDED FILE (markdown): SOURCE core/ENV/restrictions.md -->
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

### Names and Values

Generally speaking, each name or value is a ```String```.

<details>
<summary>Details</summary>
<p>
Each name or value must be one of the following:
<ul>
<li>A <code>String</code>.</li>
<li>
An object that responds to <code>#to_str</code> by returning a <code>String</code>,
which will be used as the name or value.
</li>
</ul>
A name may not contain the <code>=</code> character.
</details>
<!-- <<<<<< END INCLUDED FILE (markdown): SOURCE core/ENV/restrictions.md -->

### Demonstrations

To demonstrate, we'll use Ruby and the Ruby Interactive Shell, <code>irb</code>, beginning with their versions:

```ruby
p `ruby --version`.chomp
"ruby 2.6.4p104 (2019-08-28 revision 67798) [x64-mingw32]"
p `irb --version`.chomp
"irb 1.0.0 (2018-12-18)"
```
<!-- <<<<<< END INCLUDED FILE (markdown): SOURCE include_files/begin_irb.md -->

### Contents
- [The Basics](#the-basics)
- [Setting an Environment Variable](#setting-an-environment-variable)
- [Getting an Environment Variable](#getting-an-environment-variable)
- [Setter Methods](#setter-methods)
  - [Method #[]=](#method-)
  - [Method #store](#method-store)
  - [Method #update](#method-update)
  - [Method #replace](#method-replace)
  - [Method #delete](#method-delete)
  - [Method #clear](#method-clear)
  - [Method #shift](#method-shift)
- [Getter Methods](#getter-methods)
  - [Method #[]](#method-)
  - [Method #fetch](#method-fetch)
  - [Method #key](#method-key)
  - [Method #keys](#method-keys)
  - [Method #assoc](#method-assoc)
  - [Method #values](#method-values)
  - [Method #values_at](#method-values_at)
  - [Method #invert](#method-invert)
  - [Method #rassoc](#method-rassoc)
  - [Method #slice](#method-slice)
  - [Method #to_a](#method-to_a)
  - [Method #to_h](#method-to_h)
  - [Method #to_hash](#method-to_hash)
  - [Method #to_s](#method-to_s)
  - [Method #inspect](#method-inspect)
- [Enumeration Methods](#enumeration-methods)
  - [Method #delete_if](#method-delete_if)
  - [Method #each](#method-each)
  - [Method #each_key](#method-each_key)
  - [Method #each_pair](#method-each_pair)
  - [Method #each_value](#method-each_value)
  - [Method #filter](#method-filter)
  - [Method #filter!](#method-filter-)
  - [Method #keep_if](#method-keep_if)
  - [Method #reject](#method-reject)
  - [Method #reject!](#method-reject-)
  - [Method #select](#method-select)
  - [Method #select!](#method-select-)
- [Block-Oriented Methods](#block-oriented-methods)
  - [Method #delete_if](#method-delete_if)
  - [Method #each](#method-each)
  - [Method #each_key](#method-each_key)
  - [Method #each_pair](#method-each_pair)
  - [Method #each_value](#method-each_value)
  - [Method #fetch](#method-fetch)
  - [Method #filter](#method-filter)
  - [Method #filter!](#method-filter-)
  - [Method #keep_if](#method-keep_if)
  - [Method #reject](#method-reject)
  - [Method #reject!](#method-reject-)
  - [Method #select](#method-select)
  - [Method #select!](#method-select-)
- [Query Methods](#query-methods)
  - [Method #empty?](#method-empty)
  - [Method #has_key?](#method-has_key)
  - [Method #has_value?](#method-has_value)
  - [Method #include?](#method-include)
  - [Method #length](#method-length)
  - [Method #key?](#method-key)
  - [Method #member?](#method-member)
  - [Method #size](#method-size)
  - [Method #value?](#method-value)
- [More](#more)

### The Basics

### Setting an Environment Variable

Set environment variable <code>foo</code>:

```ruby
p ENV['foo'] = '0'
"0"
```

### Getting an Environment Variable

Get environment variable <code>foo</code>:

```ruby
p ENV['foo']
"0"
```

### Setter Methods

#### Method #[]=

```ruby
ENV[name] = value
```

Use <code>ENV#[]=</code> to create, update, or delete an environment variable.
The method returns the environment variable's value.

Create an environment variable:

```ruby
ENV.delete('foo')
p ENV['foo'] = '0'
"0"
p ENV['foo']
"0"
```

Update an environment variable:

```ruby
p ENV['foo'] = '1'
"1"
p ENV['foo']
"1"
```

Delete an environment variable by setting its value to ```nil```:

```ruby
p ENV['foo'] = nil
nil
p ENV['foo']
nil
```

Delete a non-existent environment variable (raises no exception):

```ruby
p ENV['foo'] = nil
nil
```

Give a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV[Object.new] = '0'
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

Give a value that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV['foo'] = Object.new
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

Give a name that's not a non-allowed ```String``` (raises ```Errno::EINVAL```):

```ruby
begin
    ENV['foo='] = '0'
  rescue => x
    p x
  end
#<Errno::EINVAL: Invalid argument - ruby_setenv(foo=)>
```

#### Method #store

```ruby
ENV.store(name, value)
```

Method <code>ENV#store</code> is an alias for method <code>ENV#[]=</code>.

Use <code>ENV#store</code> to create, update, or delete an environment variable.
The method returns the environment variable's value.

Create an environment variable:

```ruby
ENV.delete('foo')
p ENV.store('foo', '0')
"0"
p ENV['foo']
"0"
```

Update an environment variable:

```ruby
p ENV.store('foo', '1')
"1"
p ENV['foo']
"1"
```

Delete an environment variable by setting its value to ```nil```:

```ruby
p ENV.store('foo', nil)
nil
p ENV['foo']
nil
```

Delete a non-existent environment variable (raises no exception):

```ruby
p ENV.store('foo', nil)
nil
```

Give a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.store(Object.new, '0')
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

Give a value that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.store('foo', Object.new)
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

Give a name that's not a non-allowed ```String``` (raises ```Errno::EINVAL```):

```ruby
begin
    ENV.store('foo=',  '0')
  rescue => x
    p x
  end
#<Errno::EINVAL: Invalid argument - ruby_setenv(foo=)>
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
<!-- <<<<<< END GENERATED FILE (include): SOURCE core/ENV/template.md -->
