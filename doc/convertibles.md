## Class-Convertible Objects

Some Ruby methods accept one or more objects
that can be either:
* _Of a given class_, and so accepted as is.
* _Convertible to that class_, in which case
  the called method converts the object.

For the relevant classes, the conversion is done by calling
the following

| Target Class  | Conversion Method |
| ------------- | ------------- |
| <tt>Array</tt>  | </tt>to_ary</tt>  |
| <tt>Hash</tt>  | </tt>to_hash</tt>  |
| <tt>Integer</tt>  | </tt>to_int</tt>  |
| <tt>String</tt>  | </tt>to_str</tt>  |

### Array-Convertible Objects

An _Array-convertible object_ is an object that:
* Has an instance method <tt>to_ary</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Array)</tt> returns <tt>true</tt>.

The examples in this section use method <tt>Array#replace</tt>,
which accepts an Array-convertible argument.

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

A _Hash-convertible object_ is an object that:
* Has an instance method <tt>to_hash</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Hash)</tt> returns <tt>true</tt>.

The examples in this section use method <tt>Hash#merge</tt>,
which accepts a Hash-convertible argument.

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

An _Integer-convertible object_ is an object that:
* Has an instance method <tt>to_int</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(Integer)</tt> returns <tt>true</tt>.

The examples in this section use method <tt>Array.new</tt>,
which accepts an Integer-convertible argument.

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

Integer-convertible builtin classes include:
* Integer
* Bignum
* Fixnum
* Float
* Complex
* Rational

### String-Convertible Objects

A _String-convertible object_ is an object that:
* Has an instance method <tt>to_str</tt>.
* The method accepts no arguments.
* The method returns an object <tt>obj</tt> for which <tt>obj.kind_of?(String)</tt> returns <tt>true</tt>.

The examples in this section use method <tt>String::new</tt>,
which accepts a String-convertible argument.

This class is String-convertible:

```ruby
class StringConvertible
  def to_str
    'foo'
  end
end
String.new(StringConvertible.new) # => "foo"
```

This class is not String-convertible (no <tt>to_str</tt> method):

```ruby
class NoToStr; end
String.new(NoToStr.new) # Raises TypeError (no implicit conversion of NoToStr into String)
```

This class is not String-convertible (method <tt>to_str</tt> takes arguments):

```ruby
class ToStrTakesArguments
  def to_str(x)
    'foo'
  end
end
String.new(ToStrTakesArguments.new) # Raises ArgumentError (wrong number of arguments (given 0, expected 1))
```

This class is not String-convertible (method <tt>to_str</tt> returns non-String):

```ruby
class ToStrReturnsNonString
  def to_str
    :foo
  end
end
String.new(ToStrReturnsNonString.new) # Raises TypeError (can't convert ToStrReturnsNonString to String (ToStrReturnsNonString#to_str gives Symbol))
```
