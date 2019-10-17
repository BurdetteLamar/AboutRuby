<!-- >>>>>> BEGIN GENERATED FILE (include): SOURCE std-lib/OpenStruct/template.md -->
<!-- >>>>>> BEGIN INCLUDED FILE (markdown): SOURCE include_files/begin_irb.md -->
## OpenStruct

### What's an OpenStruct?

From the documentation for Ruby's <code>OpenStruct/code> class:

>An OpenStruct is a data structure, similar to a Hash, that allows the definition of arbitrary attributes with their accompanying values.

It's much like a <code>Struct</code>, but with more bells and whistles: you can, for example, add and delete attributes.

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
- [Accessing Attributes](#accessing-attributes)
- [Iteration](#iteration)
- [More Ways to Create an OpenStruct Object](#more-ways-to-create-an-openstruct-object)
- [Equality](#equality)
- [More](#more)

### The Basics

Make OpenStruct available (and show its superclass):

```ruby
require 'ostruct'
p OpenStruct.superclass
Object
```

Create an OpenStruct object, then display it (method <code>:to_s</code> is an alias of method <code>:inspect</code>):

```ruby
person = OpenStruct.new(:name => 'Burdette Lamar', :city => 'Houston', :state => 'TX')
puts person.to_s
#<OpenStruct name="Burdette Lamar", city="Houston", state="TX">
```

An undefined attribute returns nil, and does not raise an exception:

```ruby
p person.country
nil
```

Add an attribute, by assigning to it:

```ruby
person.country = 'USA'
p person.country
"USA"
```

Remove an attribute (method <code>:delete_field</code> returns the deleted value):

```ruby
p person.delete_field(:country)
"USA"
p person
#<OpenStruct name="Burdette Lamar", city="Houston", state="TX">
```

Setting a attribute's value to nil does not delete the attribute.

```ruby
person.state = nil
p person
#<OpenStruct name="Burdette Lamar", city="Houston", state=nil>
```

### Accessing Attributes

Create an attribute with an accessor method, or method <code>:[]=</code> with an augument (symbol or string):

```ruby
p person.hair_color = :gray
:gray
p person[:eye_color] = :brown
:brown
p person['hair_style'] = :ponytail
:ponytail
```

Get an attribute's value with its accessor method or method <code>:[]</code>:

```ruby
p person.hair_color
:gray
p person[:eye_color]
:brown
p person['hair_style']
:ponytail
```

Change an attribute's value with its accessor method or method <code>:[]=</code>:

```ruby
p person.hair_color = :silver
:silver
p person[:eye_color] = :dark_brown
:dark_brown
p person['hair_style'] = :tie_back
:tie_back
```

Weird names are allowed:

```ruby
p person[:'Weight (pounds)'] = 191
191
p person[:'Weight (pounds)']
191
p person['Weight (pounds)']
191
p person.send(:'Weight (pounds)')
191
```

Get all attributes as a hash.

```ruby
p person.to_h
{:name=>"Burdette Lamar", :city=>"Houston", :state=>nil, :hair_color=>:silver, :eye_color=>:dark_brown, :hair_style=>:tie_back, :"Weight (pounds)"=>191}
```

Process all attributes into a hash:

```ruby
p person.to_h {|name, value| [name.to_s, value.to_s] }
{"name"=>"Burdette Lamar", "city"=>"Houston", "state"=>"", "hair_color"=>"silver", "eye_color"=>"dark_brown", "hair_style"=>"tie_back", "Weight (pounds)"=>"191"}
```

A new OpenStruct instance has no methods:

```ruby
person = OpenStruct.new(:name => 'Burdette Lamar', :city => 'Houston', :state => 'TX')
p person.methods(false)
[]
```

Accessing an attribute with method <code>:[]</code> does not create accessor methods:

```ruby
p person[:name]
"Burdette Lamar"
p person['name']
"Burdette Lamar"
p person.methods(false)
[]
```

But accessing it with method <code>:[]=</code> does create accessor methods:

```ruby
person[:name] = 'Lamar, Burdette'
p person.methods(false)
[:name=, :name]
```

As does accessing an attribute with either of its accessor methods:

```ruby
p person.state
"TX"
p person.methods(false)
[:state=, :name, :state, :name=]
person.city = 'Boston'
p person.methods(false)
[:state=, :city=, :name, :city, :state, :name=]
```

Dig for content in objects that support method <code>:dig</code>:

```ruby
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
{:baz=>"Bag"}
```

Freeze an OpenStruct object:

```ruby
ostruct = OpenStruct.new(:a => 0)
p ostruct.frozen?
false
ostruct.freeze
p ostruct.frozen?
true
```

For the Marshal library:

```ruby
ostruct = OpenStruct.new(:a => 0, :b => 1)
p ostruct
#<OpenStruct a=0, b=1>
x = ostruct.marshal_dump
p x
{:a=>0, :b=>1}
ostruct.marshal_load(x)
p ostruct
#<OpenStruct a=0, b=1>
```

### Iteration

Iterate with method <code>:each_pair</code>:

```ruby
person.each_pair do |name, value|
      p [name, value]
  end
[:name, "Lamar, Burdette"]
[:city, "Boston"]
[:state, "TX"]
```

Get an enumerator for the attributes.:

```ruby
enum = person.each_pair
p enum
#<Enumerator: #<OpenStruct name="Lamar, Burdette", city="Boston", state="TX">:each_pair>
```

### More Ways to Create an OpenStruct Object

Create an OpenStruct from a Struct:

```ruby
Person = Struct.new(:name, :city, :state)
struct = Person.new('Burdette Lamar', 'Houston', 'TX')
ostruct = OpenStruct.new(struct)
p ostruct
#<OpenStruct name="Burdette Lamar", city="Houston", state="TX">
```

Create an OpenStruct from another OpenStruct:

```ruby
another_ostruct = OpenStruct.new(ostruct)
p another_ostruct
#<OpenStruct name="Burdette Lamar", city="Houston", state="TX">
```

Create an empty OpenStruct:

```ruby
ostruct = OpenStruct.new
p ostruct
#<OpenStruct>
```

### Equality

A couple of OpenStruct instances to compare:

```ruby
a = OpenStruct.new(:a => 0, :b => 1)
b = OpenStruct.new(:b => 1, :a => 0)
p a
#<OpenStruct a=0, b=1>
p b
#<OpenStruct b=1, a=0>
```

Methods <code>:==</code> and <code>:eql?</code> test equality.

```ruby
p a == b
true
p a.eql?(b)
true
```

Method <code>:hash</code> returns an integer hash value, not a hash.  The value varies from run to run, so here we just show that it's an integer:

```ruby
p a.hash.instance_of?(Integer)
true
p b.hash.instance_of?(Integer)
true
p a.hash == b.hash
true
```

### More

- [Documentation](https://ruby-doc.org/stdlib-2.6.3/libdoc/ostruct/rdoc/OpenStruct.html)
- [Source code](https://github.com/ruby/ruby/blob/master/lib/ostruct.rb)
- [Related gems](https://rubygems.org/search?query=openstruct)
- [Performance](https://www.google.com/search?q=ruby+openstruct+performance)
<!-- <<<<<< END GENERATED FILE (include): SOURCE std-lib/OpenStruct/template.md -->
