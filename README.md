qyaml
=====

Functions to convert javascript objects to/from simple YAML notation.  Supports the
basics, hashes of `name: value` pairs and lists of `- ` entities.  Hashes and arrays
can contain other hashes and/or arrays.

    const qyaml = require('qyaml');
    const yaml =
        "a: 1" +
        "b: two three" +
        "c:" +
        "  - 4" +
        "  - 5" +
        "";

    let obj = qyaml.decode(yaml);
    // => { a: 1, b: 'two three', c: [ 4, 5 ] }


Api
---

### qyaml.decode( yamlString )

Decode the yaml string into an object.  Throws on yaml error.

### qyaml.encode( objectOrArray )

Encode the object (or array) into a multi-line yaml string.  Lines are terminated with
newlines `'\n'`.

### coder = qyaml.defaults( options )

Return a new yaml encoder/decoder configured for the given options.  The coder has
methods `decode`, `encode` and `defaults`.  Options are inherited, thus
`qyaml.defaults({ a: 1 }).defaults({ b: 1 })` will create a coder with two options `a`
and `b` set.

Recognized options:

- `indent` - number of spaces for each nesting level.  Default 2.


YAML Syntax
-----------

Qyaml recognizes a small subset of the [YAML syntax](https://yaml.org/spec/1.2/spec.html),
just enough to cover what JSON handles -- simple scalars, UTF8 strings, objects, arrays.
And comment lines.

Only compound data may be converted:  lists and hashes.  All simple scalars must be
contained as entities in a list or as named values in a hash.

lists
  values
  anonymous lists, hashes
  (abbreviated lists aka "flow collections" [...] are not supported)

hashes
  names
  values
  (abbreviated hashes aka "flow collections" {...} are not supported)

comments

whitespace
  indentation
  blank lines

numbers
  base10, o8, x16, float, exponential notation (converted with Number())
  (numbers are parsed as numbers, not strings.  So 000 is the value `0`, not a string "000")

strings
  space-delimited
  quoted (json-stringified doublequoted strings only, no singlequote handling)
  (no line continuations)

built-in types
  date
  regexp
  Number String Boolean NaN Infinity


Change Log
----------

- 0.1.0 - initial version


Todo
----

- trim trailing comments
- support multi-line strings
- optionally recognize yes/no on/off as booleans
- support single-quoted strings (for compat)
- support literal block scalars


Related Work
------------
