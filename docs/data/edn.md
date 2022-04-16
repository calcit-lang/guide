# Cirru Extensible Data Notation

> Data notation based on Cirru. Learnt from [Clojure EDN](https://github.com/edn-format/edn).

EDN data is designed to be transferred across networks are strings. 2 functions involved:

- `parse-cirru-edn`
- `format-cirru-edn`

although items of a HashSet nad fields of a HashMap has no guarantees, they are being formatted with an given order in order that its returns are reasonably stable.

### Liternals

For literals, if written in text syntax, we need to add `do` to make sure it's a line:

```cirru
do nil
```

for a number:

```cirru
do 1
```

for a symbol:

```cirru
do 's
```

there's also keyword:

```cirru
do :k
```

### String escaping

for a string:

```cirru
do |demo
```

or wrap with double quotes to support special characters like spaces:

```cirru
do "|demo string"
```

or use a single double quote for mark strings:

```cirru
do "\"demo string"
```

`\n` `\t` `\"` `\\` are supported.

### Data structures:

for a list:

```cirru
[] 1 2 3
```

or nested list inside list:

```cirru
[] 1 2
  [] 3 4
```

HashSet for unordered elements:

```cirru
#{} :a :b :c
```

HashMap:

```cirru
{}
  :a 1
  :b 2
```

also can be nested:

```cirru
{}
  :a 1
  :c $ {}
    :d 3
```

Also a record:

```cirru
%{} :A
  :a 1
```

### Quotes

For quoted data, there's a special semantics for representing them, since that was neccessary for `compact.cirru` usage, where code lives inside a piece of data, marked as:

```cirru
quote $ def a 1
```

at runtime, it's represented with tuples:

```cirru
:: 'quote $ [] |def |a |1
```

which means you can eval:

```bash
$ cr -e "println $ format-cirru-edn $ :: 'quote $ [] |def |a |1"

quote $ def a 1

took 0.027ms: nil
```

and also:

```bash
$ cr -e 'parse-cirru-edn "|quote $ def a 1"'
took 0.011ms: (:: 'quote ([] |def |a |1))
```

This is not a generic solution, but tuple is a special data structure in Calcit and can be used for marking up different types of data.

### Buffers

there's a special syntax for representing buffers in EDN using pairs of Hex digits as `u8`:

```cirru
buf 03 55 77 ff 00
```

which corresponds to:

```cirru
&buffer 0x03 0x55 0x77 0xff 0x00
```
