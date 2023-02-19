# Liteform

Liteform is a data interchange format with efficient syntax. It supports many data types, keys (constants), comments and more.

Here's a demonstration that covers all features of Liteform:

```lf
\ This is an inline comment
\(
  This is a multi-line comment
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

These two examples are faulty. They include both items and key-value pairs under theh same indentation level:

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

where the key-value pairs are put under an anonymous table.

## Escape Sequences

* `\\` — `\`
* `\'` — `'`
* `\"` — `"`
* `\u`*`hh`* — the unicode character with the hexadecimal code *`hh`*
* `\u(`*`...`*`)` — the unicode character with the code from the number literal *`...`*
* `\ub(`*`...`*`)` — the unicode character with the code from the binary literal *`...`*
* `\uo(`*`...`*`)` — the unicode character with the code from the octal *`...`*
* `\ux(`*`...`*`)` — the unicode character with the code from the hexadecimal *`...`*
* `\0` — NUL
* `\h` — SOH
* `\x` — STX
* `\X` — ETX
* `\T` — EOT
* `\q` — ENQ
* `\k` — ACK
* `\a` — BEL
* `\b` — BS
* `\t` — TAB
* `\n` — LF
* `\v` — VT
* `\f` — FF
* `\r` — CR
* `\o` — SO
* `\i` — SI
* `\l` — DLE
* `\1` — DC1
* `\2` — DC2
* `\3` — DC3
* `\4` — DC4
* `\K` — NAK
* `\s` — SYN
* `\B` — ETB
* `\c` — CAN
* `\m` — EM
* `\S` — SUB
* `\e` — ESC
* `\F` — FS
* `\G` — GS
* `\R` — RS
* `\U` — US
* `\d` — DEL

## Notes

* The first indentation, whether a tab or a series of spaces, is taken as the only valid indentation throughout the whole document
* All Unicode letters are supported in key names
