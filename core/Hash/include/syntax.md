### Hash Data Syntax

Until its version 1.9,
Ruby supported only the "hash rocket" syntax for Hash data:

```ruby
h = {:foo => 0, :bar => 1, :baz => 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

(The "hash rocket" is <tt>=></tt>,
sometimes in other languages called the "fat comma.")


Beginning with version 1.9,
you can write a Hash key that's a Symbol in a JSON-style syntax,
where each bareword becomes a Symbol:

```ruby
h = {foo: 0, bar: 1, baz: 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

You can also use Strings instead of barewords:

```ruby
h = {'foo': 0, 'bar': 1, 'baz': 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

And you can mix the styles:

```ruby
h = {foo: 0, :bar => 1, 'baz': 2}
h # => {:foo=>0, :bar=>1, :baz=>2}
```

But it's an error to try the JSON-style syntax
for a key that's not a bareword or a String:

```ruby
h = {0: 'zero'} # Raises SyntaxError (syntax error, unexpected ':', expecting =>)

```
