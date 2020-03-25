## Convertibles

### Array-Convertible Objects

Some methods accept one or more Array-convertible objects as arguments.

Here, "Array-convertible object" means an object that:
* Has an instance method <tt>to_ary</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Array)</tt> returns <tt>true</tt>.

This class is Array-convertible:

```ruby
class ArrayConvertible
  def to_ary
    [:foo, 'bar', baz = 2]
  end
end
a = []
a.replace(ArrayConvertible.new) # => [:foo, "bar", 2]
```

This class is not Array-convertible (no <tt>to_ary</tt> method):

```ruby
class NoToAry; end
a = []
a.replace(NoToAry.new) # Raises TypeError (no implicit conversion of NoToAry into Array)
```

This class is not Array-convertible (method <tt>to_ary</tt> takes arguments):

```ruby
class ToAryTakesArguments
  def to_ary(x)
    [:foo, 'bar', baz = 2]
  end
end
a = []
a.replace(ToAryTakesArguments.new) # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

This class is not Array-convertible (method <tt>to_ary</tt> returns non-Array):

```ruby
class ToAryReturnsNonArray
  def to_ary
    :foo
  end
end
a = []
a.replace(ToAryReturnsNonArray.new) # Raises TypeError (can't convert ToAryReturnsNonArray to Array (ToAryReturnsNonArray#to_ary gives Symbol))
```

### Hash-Convertible Objects

Some methods accept one or more Hash-convertible objects as arguments.

Here, "Hash-convertible object" means an object that:
* Has an instance method <tt>to_hash</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Array)</tt> returns <tt>true</tt>.

This class is Hash-convertible:

```ruby
class HashConvertible
  def to_hash
    {foo: 0, bar: 1, baz: 2}
  end
end
h = {}
h.merge(HashConvertible.new) # => {:foo=>0, :bar=>1, :baz=>2}
```

This class is not Hash-convertible (no <tt>to_hash</tt> method):

```ruby
class NoToHash; end
h = {}
h.merge(NoToHash.new) # Raises TypeError (no implicit conversion of NoToHash into Hash)
```

This class is not Hash-convertible (method <tt>to_hash</tt> takes arguments):

```ruby
class ToHashTakesArguments
  def to_hash(x)
    {foo: 0, bar: 1, baz: 2}
  end
end
h = {}
h.merge(ToHashTakesArguments.new) # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

This class is not Hash-convertible (method <tt>to_hash</tt> returns non-Hash):

```ruby
class ToHashReturnsNonHash
  def to_hash
    :foo
  end
end
h = {}
h.merge(ToHashReturnsNonHash.new) # Raises TypeError (can't convert ToHashReturnsNonHash to Hash (ToHashReturnsNonHash#to_hash gives Symbol))
```

### Integer-Convertible Objects

Some methods accept one or more Integer-convertible objects as arguments.

Here, "Integer-convertible object" means an object that:
* Has an instance method <tt>to_int</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Integer)</tt> returns <tt>true</tt>.

Integer-convertible builtin classes include:
* Integer
* Bignum
* Fixnum
* Float
* Complex
* Rational

This user-defined class is Integer-convertible:

```ruby
class IntegerConvertible
  def to_int
    3
  end
end
a = Array.new(IntegerConvertible.new).size
a # => 3
```

This class is not Integer-convertible (method <tt>to_int</tt> takes arguments):

```ruby
class NotIntegerConvertible
  def to_int(x)
    3
  end
end
Array.new(NotIntegerConvertible.new) # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

This class is not Integer-convertible (method <tt>to_int</tt> returns non-Integer):

```ruby
class NotIntegerConvertible
  def to_int
    :foo
  end
end
Array.new(NotIntegerConvertible.new) # Raises TypeError (can't convert NotIntegerConvertible to Integer (NotIntegerConvertible#to_int gives Symbol))
```

