<!-- >>>>>> BEGIN GENERATED FILE (include): SOURCE core/Exception/template.md -->
<!-- >>>>>> BEGIN INCLUDED FILE (markdown): SOURCE include_files/begin_irb.md -->
## Exception

### What's an Exception?

From the documentation for Ruby's <code>Exception</code> class:

>Descendants of class Exception are used to communicate between Kernel#raise and <code>rescue</code> statements in <code>begin ... end</code> blocks. Exception objects carry information about the exception – its type (the exception's class name), an optional descriptive string, and optional traceback information.

To demonstrate, we'll use Ruby and the Ruby Interactive Shell, <code>irb</code>, beginning with their versions:

```ruby
p `ruby --version`.chomp
"ruby 2.6.3p62 (2019-04-16 revision 67580) [x64-mingw32]"
p `irb --version`.chomp
"irb 1.0.0 (2018-12-18)"
```
<!-- <<<<<< END INCLUDED FILE (markdown): SOURCE include_files/begin_irb.md -->

### Contents
- [The Basics](#the-basics)
  - [Creating Exceptions](#creating-exceptions)
  - [Examining Exceptions](#examining-exceptions)
  - [Rescued Exceptions](#rescued-exceptions)
  - [Equality](#equality)
- [More Methods](#more-methods)
  - [Method :cause](#method-cause)
  - [Method :to_tty?](#method-to_tty)
- [Global Variables](#global-variables)
- [Descendant Classes](#descendant-classes)
  - [Built-In Descendant Classes](#built-in-descendant-classes)
  - [Custom Descendant Classes](#custom-descendant-classes)
- [More](#more)

### The Basics

#### Creating Exceptions

Create a new exception:

```ruby
x = Exception.new
p x
#<Exception: Exception>
```

Create a new exception with a message:

```ruby
x = Exception.new('Boo!')
p x
#<Exception: Boo!>
```

Create a new exception with a non-string argument (which is then converted to a string):

```ruby
x = Exception.new(:symbol)
p x
#<Exception: symbol>
x = Exception.new(Range.new(0, 4))
p x
#<Exception: 0..4>
```

<code>Exception.exception</code> behaves the same as <code>Exception.new</code>:

```ruby
x = Exception.exception
p x
#<Exception: Exception>
x = Exception.exception('Boo!')
p x
#<Exception: Boo!>
```

Create a new exception of the same class as an existing exception, but with a different message:

```ruby
x = Exception.exception('x message')
p x
#<Exception: x message>
y = x.exception('y message')
p y
#<Exception: y message>
p x.__id__ == y.__id__
false
```

Method <code>:exception</code> returns <code>self</code> when passed no argument:

```ruby
x = Exception.new
p x
#<Exception: Exception>
y = x.exception
p y
#<Exception: Exception>
p x.__id__ == y.__id__
true
```

Method <code>:exception</code> returns <code>self</code> when passed <code>self</code> as an argument:

```ruby
x = Exception.new
p x
#<Exception: Exception>
y = x.exception(x)
p y
#<Exception: Exception>
p x.__id__ == y.__id__
true
```

Method <code>:exception</code> returns a new exception with a new message when passed anything else as an argument:

```ruby
x = Exception.new
p x
#<Exception: Exception>
y = x.exception('Boo!')
p y
#<Exception: Boo!>
p x.__id__ == y.__id__
false
```

#### Examining Exceptions

Get a string showing an exception's class and message:
 
```ruby
x = Exception.new('Boo!')
p x.inspect
"#<Exception: Boo!>"
```

Get an exception's message:

```ruby
p x.to_s
"Boo!"
```

Get an exception's message:

```ruby
p x.message
"Boo!"
```

For an exception with no message, method <code>:message</code> returns the class name.

```ruby
x = Exception.new
p x.message
"Exception"
```

#### Rescued Exceptions

Rescue an exception:

```ruby
rescued = nil
begin
    raise Exception.new('Boo!')
  rescue Exception => x
    rescued = x
  end
```

Show its class and message:

```ruby
```
  p rescued.class
  p rescued.message
```

Method <code>:backtrace</code> returns an array of strings.  This one is large:

```ruby
  backtrace = rescued.backtrace
  p backtrace.class
Array
  p backtrace.size
20
```
  The whole thing:

```ruby
  puts backtrace
irb_input:146:in `irb_binding'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/workspace.rb:85:in `eval'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/workspace.rb:85:in `evaluate'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/context.rb:385:in `evaluate'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:493:in `block (2 levels) in eval_input'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:647:in `signal_status'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:490:in `block in eval_input'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:246:in `block (2 levels) in each_top_level_statement'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:232:in `loop'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:232:in `block in each_top_level_statement'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:231:in `catch'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:231:in `each_top_level_statement'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:489:in `eval_input'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:428:in `block in run'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:427:in `catch'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:427:in `run'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:383:in `start'
C:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/irb-1.0.0/exe/irb:11:in `<top (required)>'
C:/Ruby26-x64/bin/irb.cmd:31:in `load'
C:/Ruby26-x64/bin/irb.cmd:31:in `<main>'
```

Set backtrace, in this case to an empty array:

```ruby
  rescued.set_backtrace([])
  puts rescued.backtrace
```

The original backtrace information is still available via method <code>:backtrace_locations</code>, but the result is an array of <code>Thread::Backtrace::Location</code>, not an array of <code>String</code>.

```ruby
  p rescued.backtrace_locations.first.class
Thread::Backtrace::Location
  puts rescued.backtrace_locations
irb_input:146:in `irb_binding'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/workspace.rb:85:in `eval'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/workspace.rb:85:in `evaluate'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/context.rb:385:in `evaluate'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:493:in `block (2 levels) in eval_input'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:647:in `signal_status'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:490:in `block in eval_input'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:246:in `block (2 levels) in each_top_level_statement'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:232:in `loop'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:232:in `block in each_top_level_statement'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:231:in `catch'
C:/Ruby26-x64/lib/ruby/2.6.0/irb/ruby-lex.rb:231:in `each_top_level_statement'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:489:in `eval_input'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:428:in `block in run'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:427:in `catch'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:427:in `run'
C:/Ruby26-x64/lib/ruby/2.6.0/irb.rb:383:in `start'
C:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/irb-1.0.0/exe/irb:11:in `<top (required)>'
C:/Ruby26-x64/bin/irb.cmd:31:in `load'
C:/Ruby26-x64/bin/irb.cmd:31:in `<main>'
```

#### Equality

Two exceptions are equal under <code>:==</code> if they have the same class, message, and backtrace.

All the same:

```ruby
  clone = rescued.clone
  p rescued.class == x.class
true
  p rescued.message == x.message
true
  p rescued.backtrace == x.backtrace
true
  p rescued == clone
true
```

Different class:

```ruby
  x = RuntimeError.new(rescued.message)
  x.set_backtrace(rescued.backtrace)
  p rescued.class == x.class
false
  p rescued.message == x.message
true
  p rescued.backtrace == x.backtrace
true
  p rescued == x
false
```

Different message:

```ruby
  x = rescued.exception('Foo!')
  x.set_backtrace(rescued.backtrace)
  p rescued.class == x.class
true
  p rescued.message == x.message
false
  p rescued.backtrace == x.backtrace
true
  p rescued == x
false
```

Different backtrace:

```ruby
  x = clone.exception
  backtrace = rescued.backtrace_locations.map { |x| x.to_s }
  x.set_backtrace(backtrace)
  p rescued.class == x.class
true
  p rescued.message == x.message
true
  p rescued.backtrace == x.backtrace
false
  p rescued == x
false
```

### More Methods
    
#### Method :cause

When an exception is raised, method <code>:cause</code> returns the previously-raised exception, as shown by this code from [Starr Horne](https://www.honeybadger.io/blog/nested-errors-in-ruby-with-exception-cause/):

```ruby
def fail_and_reraise
    raise NoMethodError
  rescue
    raise RuntimeError
  end

begin
    fail_and_reraise
  rescue => e
    puts "#{ e } caused by #{ e.cause }"
  end
RuntimeError caused by NoMethodError
```

#### Method :to_tty?

Determine whether an Exception will be written to a tty:

```ruby
  p Exception.to_tty?
false
```

### Global Variables

When an exception has been raised but not yet handled (in rescue, ensure, at_exit and END blocks) the global variable $! will contain the current exception and $@ contains the current exception’s backtrace.

### Descendant Classes

#### Built-In Descendant Classes

#### Custom Descendant Classes

### More

- [Documentation](https://ruby-doc.org/core-2.6.3/Exception.html)
- [Source code](https://github.com/ruby/ruby/blob/8b2e1ca10ecf92ad402decd6b1eab586eded0ddb/error.c)
- [Related gems](https://rubygems.org/search?query=exception)
- [Performance](https://www.google.com/search?q=ruby++exception+performance)

BEST PRACTICE

<!-- <<<<<< END GENERATED FILE (include): SOURCE core/Exception/template.md -->
