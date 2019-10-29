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

When you delete a name-value pair in ```ENV```,
it is immediately deleted from the environment variables.

### Names and Values

Generally speaking, each name or value is a ```String```.

<details>
<summary>Details</summary>
<ul>
<li>Each name or value must be one of the following:
<ul>
<li>A <code>String</code>.</li>
<li>
An object that responds to <code>#to_str</code> by returning a <code>String</code>,
which will be used as the name or value.
</li>
</ul>
<li>A name may not:
 <ul>
 <li>Contain the <code>=</code> character.</li>
 <li>Be the empty string.</li>
 </ul>
</ul>
</details>
<!-- <<<<<< END INCLUDED FILE (markdown): SOURCE core/ENV/restrictions.md -->

### About Ordering

The ordering of ```ENV``` content is OS-dependent.

This will be seen in:
- A ```Hash``` returned by an ```ENV``` method.
- An ```Enumerator``` returned by an ```ENV``` method.
- An ```ENV``` method that iterates over its names, values, or name-value pairs..
- The ```String``` returned by ```ENV#shift```.
- The ```String``` returned by ```ENV#inspect```.

### Demonstrations

To demonstrate, we'll use Ruby and the Ruby Interactive Shell, <code>irb</code>, beginning with their versions:

```ruby
p `ruby --version`.chomp
"ruby 2.6.4p104 (2019-08-28 revision 67798) [x64-mingw32]"
p `irb --version`.chomp
"irb 1.0.0 (2018-12-18)"
```
<!-- <<<<<< END INCLUDED FILE (markdown): SOURCE include_files/begin_irb.md -->

### About the Examples

Some methods in ```ENV``` return ```ENV``` itself.
Typically, there are many environment variables.
It's not useful to display a large ```ENV``` in the examples here,
so most example snippets begin by resetting the contents of ```ENV```
via ```ENV.replace``` or ```ENV.clear```:

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
p ENV
{"bar"=>"1", "foo"=>"0"}
ENV.clear
p ENV
{}
```

### Contents
- [The Basics](#the-basics)
  - [Setting an Environment Variable](#setting-an-environment-variable)
  - [Getting an Environment Variable](#getting-an-environment-variable)
  - [Deleting an Environment Variable](#deleting-an-environment-variable)
- [Setter Methods](#setter-methods)
  - [ENV#[]=](#env-1)
  - [ENV#store](#envstore)
  - [ENV#delete](#envdelete)
  - [ENV#update](#envupdate)
  - [ENV#replace](#envreplace)
  - [ENV#clear](#envclear)
  - [ENV#shift](#envshift)
- [Getter Methods](#getter-methods)
  - [ENV#[]](#env-2)
  - [ENV#fetch](#envfetch)
  - [ENV#key](#envkey)
  - [ENV#keys](#envkeys)
  - [ENV#assoc](#envassoc)
  - [ENV#rassoc](#envrassoc)
  - [ENV#values](#envvalues)
  - [ENV#values_at](#envvalues_at)
  - [ENV#slice](#envslice)
  - [ENV#to_a](#envto_a)
  - [ENV#to_h](#envto_h)
  - [ENV#to_hash](#envto_hash)
  - [ENV#invert](#envinvert)
  - [ENV#to_s](#envto_s)
  - [ENV#inspect](#envinspect)
- [Enumeration Methods](#enumeration-methods)
  - [ENV#delete_if](#envdelete_if)
  - [ENV#each](#enveach)
  - [ENV#each_key](#enveach_key)
  - [ENV#each_pair](#enveach_pair)
  - [ENV#each_value](#enveach_value)
  - [ENV#filter](#envfilter)
  - [ENV#filter!](#envfilter-)
  - [ENV#keep_if](#envkeep_if)
  - [ENV#reject](#envreject)
  - [ENV#reject!](#envreject-)
  - [ENV#select](#envselect)
  - [ENV#select!](#envselect-)
- [Block-Oriented Methods](#block-oriented-methods)
  - [ENV#delete_if](#envdelete_if-1)
  - [ENV#each](#enveach-1)
  - [ENV#each_key](#enveach_key-1)
  - [ENV#each_pair](#enveach_pair-1)
  - [ENV#each_value](#enveach_value-1)
  - [ENV#fetch](#envfetch-1)
  - [ENV#filter](#envfilter-1)
  - [ENV#filter!](#envfilter--1)
  - [ENV#keep_if](#envkeep_if-1)
  - [ENV#reject](#envreject-1)
  - [ENV#reject!](#envreject--1)
  - [ENV#select](#envselect-1)
  - [ENV#select!](#envselect--1)
- [Query Methods](#query-methods)
  - [ENV#empty?](#envempty)
  - [ENV#has_key?](#envhas_key)
  - [ENV#has_value?](#envhas_value)
  - [ENV#include?](#envinclude)
  - [ENV#length](#envlength)
  - [ENV#key?](#envkey-1)
  - [ENV#member?](#envmember)
  - [ENV#size](#envsize)
  - [ENV#value?](#envvalue)
- [More](#more)

### The Basics

#### Setting an Environment Variable

Set environment variable <code>foo</code>:

```ruby
ENV.clear
p ENV['foo'] = '0'
"0"
```

#### Getting an Environment Variable

Get environment variable <code>foo</code>:

```ruby
ENV.replace('foo' => '0')
p ENV['foo']
"0"
```

#### Deleting an Environment Variable

Delete environment variable <code>foo</code>:

```ruby
ENV.replace('foo' => '0')
p ENV['foo'] = nil
nil
p ENV['foo']
nil
```

### Setter Methods

#### ENV#[]=

```ruby
ENV[name] = value # => value
```

Use <code>ENV#[]=</code> to create, update, or delete an environment variable.
The method returns the environment variable's value.

Create an environment variable:

```ruby
ENV.clear
p ENV['foo'] = '0'
"0"
p ENV['foo']
"0"
```

Update an environment variable:

```ruby
ENV.replace('foo' => '0')
p ENV['foo'] = '1'
"1"
p ENV['foo']
"1"
```

Delete an environment variable by setting its value to ```nil```:

```ruby
ENV.replace('foo' => '0')
p ENV['foo'] = nil
nil
p ENV['foo']
nil
```

Delete a non-existent environment variable (raises no exception):

```ruby
ENV.clear
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

Give a string name that's not allowed (raises ```Errno::EINVAL```):

```ruby
begin
    ENV['foo='] = '0'
  rescue => x
    p x
  end
#<Errno::EINVAL: Invalid argument - ruby_setenv(foo=)>
begin
    ENV[''] = '0'
  rescue => x
    p x
  end
#<Errno::EINVAL: Invalid argument - ruby_setenv()>
```

#### ENV#store

```ruby
ENV.store(name, value) # => value
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

Give a string name that's not allowed (raises ```Errno::EINVAL```):

```ruby
begin
    ENV.store('foo=',  '0')
  rescue => x
    p x
  end
#<Errno::EINVAL: Invalid argument - ruby_setenv(foo=)>
begin
    ENV.store('',  '0')
  rescue => x
    p x
  end
#<Errno::EINVAL: Invalid argument - ruby_setenv()>
```

#### ENV#delete

```ruby
ENV.delete(name) # => value
```

Use method <code>ENV#delete</code> to delete an environment variable.

The method returns the value of the deleted environment variable.

Delete an existing environment variable:

```ruby
ENV.replace('foo' => '0')
p ENV.delete('foo')
"0"
p ENV['foo']
nil
```

Give a name that's not an environment variable:

```ruby
ENV.clear
p ENV.delete('foo')
nil
```

Give a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.delete(Object.new)
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
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

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
ENV.update('baz' => '2', 'bat' => '3')
p ENV
{"bar"=>"1", "bat"=>"3", "baz"=>"2", "foo"=>"0"}
```

Update environment variables:

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
ENV.update('foo' => '2', 'bar' => '3')
p ENV
{"bar"=>"3", "foo"=>"2"}
```

Delete environment variables:

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
ENV.update('foo' => nil, 'bar' => nil)
p ENV
{}
```

Do all three at once;

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
ENV.update('baz' => '2', 'bar' => '3', 'foo' => nil)
p ENV
{"bar"=>"3", "baz"=>"2"}
```

Give a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.update(Object.new => '0')
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
p ENV
{"bar"=>"3", "baz"=>"2"}
```

Give a value that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.update('foo' => Object.new)
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
p ENV
{"bar"=>"3", "baz"=>"2"}
```

The pairs in the given ```Hash``` are processed in the order given.
Processing continues to the last pair, or until an error occurs.
Whan an error occurs, the processing stops, and no further changes are made.

Give a non-first name that's not a ```String``` (raises ```TypeError``` and processing stops):

```ruby
begin
    ENV.update('foo' => '1', Object.new => '0', 'baz' => '2')
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
p ENV
{"bar"=>"3", "baz"=>"2", "foo"=>"1"}
```

Give a non-first value that's not a ```String``` (raises ```TypeError``` and processing stops):

```ruby
begin
    ENV.update('foo' => '0', 'bar' => Object.new, 'baz' => '2')
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
p ENV
{"bar"=>"3", "baz"=>"2", "foo"=>"0"}
```

#### ENV#replace

```ruby
ENV.replace(hash) # => ENV
```

Use method <code>ENV#replace</code> to replace all environment variables with new ones.

The method accepts a ```Hash``` argument of name-value pairs,
and returns <code>ENV</code>.

Replace ```ENV``` with data from a ```Hash```:

```ruby
ENV.replace('baz' => '0', 'bat' => '1')
p ENV
{"bat"=>"1", "baz"=>"0"}
```
Give an argument that's not a ```Hash``` (raises ```TypeError```:

```ruby
begin
    ENV.replace(Object.new)
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into Hash>
```

Give a ```Hash``` with a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.replace(Object.new => '0')
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

Give a ```Hash``` with a value that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.replace('foo' => Object.new)
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

#### ENV#clear

```ruby
ENV.clear # => ENV
```

Use method ```ENV#clear``` to remove all environment variables.

The method returns ```ENV```.

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
p ENV.clear
{}
p ENV
{}
```

#### ENV#shift

```ruby
ENV.shift # => [name, value]
```

Use method ```ENV#shift``` to remove and return the first environment variable.

The method returns a 2-element ```Array``` of the removed name and value.

```ruby
ENV.replace('foo' => '0', 'bar' => '1')
p ENV.shift
["bar", "1"]
p ENV
{"foo"=>"0"}
```

### Getter Methods

#### ENV#[]

```ruby
ENV[name] # => value
```

Get the value of an environment variable:

```ruby
ENV['foo'] = '0'
p ENV['foo']
"0"
```

Give a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV[Object.new]
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

#### ENV#fetch

```ruby
ENV.fetch(name) # => value
```

Get the value of an environment variable:

```ruby
ENV['foo'] = '0'
p ENV.fetch('foo')
"0"
```

Give a name that's not an environment variable name (raises ```KeyError```):

```ruby
begin
    ENV.fetch('foot')
  rescue => x
    p x
  end
#<KeyError: key not found: "foot">
```

Give a name that's not a ```String``` (raises ```TypeError```):

```ruby
begin
    ENV.fetch(Object.new)
  rescue => x
    p x
  end
#<TypeError: no implicit conversion of Object into String>
```

#### ENV#key

```ruby
ENV.key(value) # => name
```

#### ENV#keys
#### ENV#assoc
#### ENV#rassoc
#### ENV#values
#### ENV#values_at
#### ENV#slice
#### ENV#to_a
#### ENV#to_h
#### ENV#to_hash
#### ENV#invert
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
<!-- <<<<<< END GENERATED FILE (include): SOURCE core/ENV/template.md -->
