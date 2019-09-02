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
p x.__id__
y = x.exception('y message')
p y
p y.__id__
p x.__id__ == y.__id__
```

Method <code>:exception</code> returns <code>self</code> when passed no argument:

```#run_irb
x = Exception.new
p x
p x.__id__
y = x.exception
p y
p y.__id__
p x.__id__ == y.__id__
```

Method <code>:exception</code> returns <code>self</code> when passed <code>self</code> as an argument:

```#run_irb
x = Exception.new
p x
p x.__id__
y = x.exception(x)
p y
p y.__id__
p x.__id__ == y.__id__
```

Method <code>:exception</code> returns a new exception with a new message when passed anything else as an argument:

```#run_irb
x = Exception.new
p x
p x.__id__
y = x.exception('Boo!')
p y
p y.__id__
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

#### Backtraces

    #backtrace
    #backtrace_locations
    #set_backtrace

#### Equality

#### Rescuing Exceptions
    
### More Methods
    
#### Method :cause

    #cause
    
    https://www.honeybadger.io/blog/nested-errors-in-ruby-with-exception-cause/
    
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

#### Method :to_tty?

    ::to_tty?

### Global Variables

When an exception has been raised but not yet handled (in rescue, ensure, at_exit and END blocks) the global variable $! will contain the current exception and $@ contains the current exception’s backtrace.

### Descendant Classes

#### Built-In Descendant Classes

####  Custom Descendant Classes

### More

- [Documentation](https://ruby-doc.org/core-2.6.3/Exception.html)
- [Source code](https://github.com/ruby/ruby/blob/8b2e1ca10ecf92ad402decd6b1eab586eded0ddb/error.c)
- [Related gems](https://rubygems.org/search?query=exception)
- [Performance](https://www.google.com/search?q=ruby++exception+performance)
