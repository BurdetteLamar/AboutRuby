## Exception

### What's an Exception?

From the documentation for Ruby's <code>Exception</code> class:

>Descendants of class Exception are used to communicate between Kernel#raise and <code>rescue</code> statements in <code>begin ... end</code> blocks. Exception objects carry information about the exception – its type (the exception's class name), an optional descriptive string, and optional traceback information.

@[:markdown](../../include_files/begin_irb.md)

@[:page_toc](### Contents)

### The Basics

    ::exception
    ::new
    #exception
    #inspect
    #to_s

#### Messages

    #full_message
    #message
    
#### Backtraces

    #backtrace
    #backtrace_locations
    #set_backtrace

### More Methods
    
####  Method :==

    #==

#### Method :cause

    #cause

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
