# Liteform

Liteform is a data interchange format with efficient syntax. It supports many data types, keys (constants), comments and more.

Here's an example:

```lf
\(
  This is a demonstration that covers all features of Liteform.
\)

ghoom \ ghoom is a table of keys and values
  name "ghoom"
  age 21 \ The 0b, 0o and 0x prefixes are supported, even for decimals
  screen_ratio 16:9
  most_used_number_base 0..9 \ This is an integer range
  favorite_interval [0 1] \ This is a mathematical interval. Parentheses are also supported. The opening and closing brackets can be of different types (square or round)
  favorite_interval_shorthand 0...1 \ x...y always expands to [x y]
  favorite_neon_color #f00 \ The alpha channel and double-digit channel values are also supported
  touches_grass_often no \ yes, no, on, off, true and false are all reserved keywords
color_array
  favorite_neon_color
  #0f0
  #00f
array_containing_anonymous_array
  .
    "amogus"
dice_roll ?1..6 \ See below for more details
weather $weather \ weather is an internal key defined as the value of the external key $weather which comes from the programming language
```

## Number Syntax

These are all valid numerals:

```lf
1
1.1
.1
1. \ which is is just 1
```

## Randomness

Liteform supports random definitions and selections of things. Here is an example that covers all types of randomness syntax:

```lf
?1..6 \ a random integer in range 1..6
?1...100 \ a random integer in interval 1...100
? \ a random item from an array
  item1
  item2
.
  ? \ a random key definition
    key1 value1
    key2 value2
```

## Fault Tolerance

These are faulty:

```lf
item1
key1 value1
key2 value2
item2
```

```lf
key1 value1
key2 value2
item1
item2
```

However, they should be (respectively) interpreted as

```lf
item1
.
  key1 value1
  key2 value2
item2
```

```lf
.
  key1 value1
  key2 value2
item1
item2
```

## Unusual Features

Variable names can start with decimal digits as long as they're followed by a letter or underscore.

```lf
2b
6502_specs
```

## Notes

* The first indentation, whether a tab or a series of spaces, is taken as the only valid indentation throughout the whole document
* All Unicode letters are supported in key names
