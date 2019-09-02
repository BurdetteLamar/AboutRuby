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
  - [Messages](#messages)
  - [Backtraces](#backtraces)
- [More Methods](#more-methods)
  - [Method :==](#method-)
  - [Method :cause](#method-cause)
  - [Method :to_tty?](#method-to_tty)
- [Global Variables $! and $A](#global-variables-and-a)
- [Descendant Classes](#descendant-classes)
  - [Built-In Descendant Classes](#built-in-descendant-classes)
  - [Custom Descendant Classes](#custom-descendant-classes)
- [More](#more)

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

### Global Variables $! and $A

When an exception has been raised but not yet handled (in rescue, ensure, at_exit and END blocks) the global variable $! will contain the current exception and $@ contains the current exception’s backtrace.

### Descendant Classes

#### Built-In Descendant Classes

####  Custom Descendant Classes

### More

- [Documentation](https://ruby-doc.org/core-2.6.3/Exception.html)
- [Source code](https://github.com/ruby/ruby/blob/8b2e1ca10ecf92ad402decd6b1eab586eded0ddb/error.c)
- [Related gems](https://rubygems.org/search?query=exception)
- [Performance](https://www.google.com/search?q=ruby++exception+performance)
<!-- <<<<<< END GENERATED FILE (include): SOURCE core/Exception/template.md -->
