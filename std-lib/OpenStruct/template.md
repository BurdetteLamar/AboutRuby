## OpenStruct

### What's an OpenStruct?

From the documentation for Ruby's OpenStruct class:

>An OpenStruct is a data structure, similar to a Hash, that allows the definition of arbitrary attributes with their accompanying values.

It's much like a Struct, but with more bells and whistles: you can, for example, add and delete attributes.

To demonstrate, I'll use Ruby and the Ruby Interactive Shell, <code>irb</code>, beginning with their versions:

```#run_irb
p `ruby --version`.chomp
p `irb --version`.chomp
```

@[:page_toc](### Contents)

### The Basics

Make OpenStruct available (and show its superclass):

```#run_irb
require 'ostruct'
p OpenStruct.superclass
```

Create an OpenStruct object, then display it (method :to_s is an alias of method :inspect):

```#run_irb
person = OpenStruct.new(:name => 'Burdette Lamar', :city => 'Houston', :state => 'TX')
puts person.to_s
```

An undefined attribute returns nil, and does not raise an exception:

```#run_irb
p person.country
```

Add an attribute, by assigning to it:

```#run_irb
person.country = 'USA'
p person.country
```

Remove an attribute (method :delete_field returns the deleted value):

```#run_irb
p person.delete_field(:country)
p person
```

Setting a attribute's value to nil does not delete the attribute.

```#run_irb
person.state = nil
p person
```

### Accessing Attributes

Create an attribute with an accessor method, or method :[]= with an augument (symbol or string):

```#run_irb
p person.hair_color = :gray
p person[:eye_color] = :brown
p person['hair_style'] = :ponytail
```

Get an attribute's value with its accessor method or method :[] with an argument:

```#run_irb
p person.hair_color
p person[:eye_color]
p person['hair_style']
```

Change an attribute's value with its accessor method or method :[]= and an argument:

```#run_irb
p person.hair_color = :silver
p person[:eye_color] = :dark_brown
p person['hair_style'] = :tie_back
```

Weird names are allowed:

```#run_irb
p person[:'Weight (pounds)'] = 191
p person[:'Weight (pounds)']
p person['Weight (pounds)']
p person.send(:'Weight (pounds)')
```

Get all attributes as a hash.

```#run_irb
p person.to_h
```

Process all attributes into a hash:

```#run_irb
p person.to_h {|name, value| [name.to_s, value.to_s] }
```

A new OpenStruct instance has no methods:

```#run_irb
person = OpenStruct.new(:name => 'Burdette Lamar', :city => 'Houston', :state => 'TX')
p person.methods(false)
```

Accessing an attribute with method :[] does not create accessor methods:

```#run_irb
p person[:name]
p person['name']
p person.methods(false)
[]
```

But accessing it with method :[]= does create accessor methods:

```#run_irb
person[:name] = 'Lamar, Burdette'
p person.methods(false)
```

As does accessing an attribute with either of its accessor methods:

```#run_irb
p person.state
p person.methods(false)
person.city = 'Boston'
p person.methods(false)
```

Dig for content in objects that support method :dig:

```#run_irb
MyStruct = Struct.new(:foo)
ostruct = OpenStruct.new(
        :bar => MyStruct.new(
              Array.new([
                                Hash.new(
                                      :baz => 'Bag'
                                )
                          ])
        )
  )
p ostruct.dig(:bar, :foo, 0, :baz)
```

Freeze an OpenStruct object:

```#run_irb
ostruct = OpenStruct.new(:a => 0)
p ostruct.frozen?
ostruct.freeze
p ostruct.frozen?
```

For the Marshal library:

```#run_irb
ostruct = OpenStruct.new(:a => 0, :b => 1)
p ostruct
x = ostruct.marshal_dump
p x
ostruct.marshal_load(x)
p ostruct
```

### Iteration

Iterate with method :each_pair:

```#run_irb
person.each_pair do |name, value|
  p [name, value]
end
```

Get an enumerator for the attributes.:

```#run_irb
enum = person.each_pair
p enum
```

### More Ways to Create an OpenStruct Object

Create an OpenStruct from a Struct:

```#run_irb
Person = Struct.new(:name, :city, :state)
struct = Person.new('Burdette Lamar', 'Houston', 'TX')
ostruct = OpenStruct.new(struct)
p ostruct
```

Create an OpenStruct from another OpenStruct:

```#run_irb
another_ostruct = OpenStruct.new(ostruct)
p another_ostruct
```

Create an empty OpenStruct:

```#run_irb
ostruct = OpenStruct.new
p ostruct
```

### Equality

A couple of OpenStruct instances to compare:

```#run_irb
a = OpenStruct.new(:a => 0, :b => 1)
b = OpenStruct.new(:b => 1, :a => 0)
p a
p b
```

Methods :== and :eql? test equality.

```#run_irb
p a == b
p a.eql?(b)
```

Method :hash returns an integer value, not a hash:

```#run_irb
p a.hash
p b.hash
```

### More

- [Documentation](https://ruby-doc.org/stdlib-2.6.3/libdoc/ostruct/rdoc/OpenStruct.html)
- [Source code](https://github.com/ruby/ruby/blob/master/lib/ostruct.rb)
- [Related gems](https://rubygems.org/search?query=openstruct)
- [Performance](https://www.google.com/search?q=ruby+openstruct+performance)
