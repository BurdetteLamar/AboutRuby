## Exception

### What's an Exception?

From the documentation for Ruby's <code>Exception</code> class:

>Descendants of class Exception are used to communicate between Kernel#raise and <code>rescue</code> statements in <code>begin ... end</code> blocks. Exception objects carry information about the exception – its type (the exception's class name), an optional descriptive string, and optional traceback information.

@[:markdown](../../include_files/begin_irb.md)

@[:page_toc](### Contents)

### The Basics

#### Creating Exceptions

##### Method Exception.new

Create an exception:

```#run_irb
x = Exception.new
p x
```

Create an exception with a message:

```#run_irb
x = Exception.new('Boo!')
p x
```

##### Method Exception.exception

<code>Exception.exception</code> behaves the same as <code>Exception#exception</code>:

```#run_irb
x = Exception.exception
p x
x = Exception.exception('Boo!')
p x
```

##### Method Exception#exception

#### Examining Exceptions

    #inspect
    #to_s

#### Rescuing Exceptions
    
#### Messages

    #message
    #full_message
    
#### Backtraces

    #backtrace
    #backtrace_locations
    #set_backtrace

### More Methods
    
####  Method :==

    #==

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
