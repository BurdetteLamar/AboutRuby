## Exception

### What's an Exception?

From the documentation for Ruby's <code>Exception</code> class:

>Descendants of class Exception are used to communicate between Kernel#raise and <code>rescue</code> statements in <code>begin ... end</code> blocks. Exception objects carry information about the exception – its type (the exception's class name), an optional descriptive string, and optional traceback information.

@[:markdown](../../include_files/begin_irb.md)

@[:page_toc](### Contents)

### The Basics

#### Creating Exceptions

Create a new exception:

```#run_irb
x = Exception.new
p x
```

Create a new exception with a message:

```#run_irb
x = Exception.new('Boo!')
p x
```

Create a new exception with a non-string argument (which is then converted to a string):

```#run_irb
x = Exception.new(:symbol)
p x
x = Exception.new(Range.new(0, 4))
p x
```

<code>Exception.exception</code> behaves the same as <code>Exception.new</code>:

```#run_irb
x = Exception.exception
p x
x = Exception.exception('Boo!')
p x
```

Create a new exception of the same class as an existing exception, but with a different message:

```#run_irb
x = Exception.exception('x message')
p x
y = x.exception('y message')
p y
p x.__id__ == y.__id__
```

Method <code>:exception</code> returns <code>self</code> when passed no argument:

```#run_irb
x = Exception.new
p x
y = x.exception
p y
p x.__id__ == y.__id__
```

Method <code>:exception</code> returns <code>self</code> when passed <code>self</code> as an argument:

```#run_irb
x = Exception.new
p x
y = x.exception(x)
p y
p x.__id__ == y.__id__
```

Method <code>:exception</code> returns a new exception with a new message when passed anything else as an argument:

```#run_irb
x = Exception.new
p x
y = x.exception('Boo!')
p y
p x.__id__ == y.__id__
```

#### Examining Exceptions

Get a string showing an exception's class and message:
 
```#run_irb
x = Exception.new('Boo!')
p x.inspect
```

Get an exception's message:

```#run_irb
p x.to_s
```

Get an exception's message:

```#run_irb
p x.message
```

For an exception with no message, method <code>:message</code> returns the class name.

```#run_irb
x = Exception.new
p x.message
```

#### Rescued Exceptions

Rescue an exception:

```#run_irb
rescued = nil
begin
  raise Exception.new('Boo!')
rescue Exception => x
  rescued = x
end
```

Show its class and message:

```#run_irb
```
  p rescued.class
  p rescued.message
```

Method <code>:backtrace</code> returns an array of strings.  This one is large:

```#run_irb
  backtrace = rescued.backtrace
  p backtrace.class
  p backtrace.size
```
  The whole thing:

```#run_irb
  puts backtrace
```

Set backtrace, in this case to an empty array:

```#run_irb
  rescued.set_backtrace([])
  puts rescued.backtrace
```

The original backtrace information is still available via method <code>:backtrace_locations</code>, but the result is an array of <code>Thread::Backtrace::Location</code>, not an array of <code>String</code>.

```#run_irb
  p rescued.backtrace_locations.first.class
  puts rescued.backtrace_locations
```

#### Equality

Two exceptions are equal under <code>:==</code> if they have the same class, message, and backtrace.

All the same:

```#run_irb
  clone = rescued.clone
  p rescued.class == x.class
  p rescued.message == x.message
  p rescued.backtrace == x.backtrace
  p rescued == clone
```

Different class:

```#run_irb
  x = RuntimeError.new(rescued.message)
  x.set_backtrace(rescued.backtrace)
  p rescued.class == x.class
  p rescued.message == x.message
  p rescued.backtrace == x.backtrace
  p rescued == x
```

Different message:

```#run_irb
  x = rescued.exception('Foo!')
  x.set_backtrace(rescued.backtrace)
  p rescued.class == x.class
  p rescued.message == x.message
  p rescued.backtrace == x.backtrace
  p rescued == x
```

Different backtrace:

```#run_irb
  x = clone.exception
  backtrace = rescued.backtrace_locations.map { |x| x.to_s }
  x.set_backtrace(backtrace)
  p rescued.class == x.class
  p rescued.message == x.message
  p rescued.backtrace == x.backtrace
  p rescued == x
```

### More Methods
    
#### Method :cause

When an exception is raised, method <code>:cause</code> returns the previously-raised exception, as shown by this code from [Starr Horne](https://www.honeybadger.io/blog/nested-errors-in-ruby-with-exception-cause/):

```#run_irb
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
```

#### Method :to_tty?

Determine whether an Exception will be written to a tty:

```#run_irb
  p Exception.to_tty?
```

### Global Variables

In a <code>rescue</code>, <code>ensure</code>, <code>at_exit</code>, or <code>END</code> block, the already-raised but not-yet-handled exception is accessible via global variables.  <code>$!</code> has the raised exception, and <code>$@</code> has its backtrace

```#run_irb
  begin
    raise Exception.new('Boo!')
  rescue Exception => x
    p $!
    puts $@
  end
```

### Descendant Classes

#### Built-In Descendant Classes

#### Custom Descendant Classes

### More

- [Documentation](https://ruby-doc.org/core-2.6.3/Exception.html)
- [Source code](https://github.com/ruby/ruby/blob/8b2e1ca10ecf92ad402decd6b1eab586eded0ddb/error.c)
- [Related gems](https://rubygems.org/search?query=exception)
- [Performance](https://www.google.com/search?q=ruby++exception+performance)

BEST PRACTICE

