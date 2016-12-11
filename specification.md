# Bintoken Specification Version 0.10

This specification uses the [Augmented Backus-Naur Form](http://en.wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_Form) with the following extension: `%xZZ` is a short-hand notation for `%x00-FF`, which represents any value between `%x00` and `%xFF`.

## Structure

Bintoken consists of a sequence of tokens, and a token consists of one or more [octets](http://en.wikipedia.org/wiki/Octet_%28computing%29). Each token starts with an octet containing a type, and some tokens may be followed by additional octets. There are four kinds of types, which are described below.

### Value types

Value types consists of a single octet that contains both the type and the value. Value types occupy three type ranges.

* The range `%x00` to `%x7F` are small positive integers that encode the values 0 to 127. For example, the value 1 is encoded as `%x01`.
* The range `%xE0` to `%xFF` are small negative integers that encode the values -32 to -1. For example, the value -1 is encoded as `%xFF`.
* The range `%x80` to `%x8F` is reserved for special values. For example, the boolean value of true is encoded as `%x81`.

Unknown value tokens can be skipped by advancing over the type octet.

### Fixed-length types

Fixed-length types require a fixed amount of octets to carry the value. The type contains information about the primitive data type of the value (e.g. integer or floating-point), and the number of octets in the value. The value must be located immediately after the type.

The primitive data type is encoded into the type by the following bit pattern.

Bit pattern | Primitive type
--- | ---
ZZZZ Z000 | Signed integer
ZZZZ Z001 | Character
ZZZZ Z010 | Float
ZZZZ Z011 | Byte
ZZZZ Z100 | Reserved for future use
ZZZZ Z101 | Reserved for future use
ZZZZ Z110 | Reserved for future use
ZZZZ Z111 | Reserved for future use

The size type is encoded into the type by the following bit patterns.

Bit pattern | Size type | Size in octets
--- | ---: | ---:
1010 0ZZZ |	8-bits |	1
1011 0ZZZ |	16-bits |	2
1100 0ZZZ |	32-bits |	4
1101 0ZZZ |	64-bits |	8

For example, the 16-bits signed integer value 4660 (0x1234) is encoded as `%xB0.12.34`.

Unknown fixed-length tokens can be skipped by advancing over the type octet plus the following 1, 2, 4, or 8 octets depending on the size type.

### Variable-length types

Variable-length types require a variable amount of octets to carry the value. Variable-length tokens are encoded as a [type-length-value](http://en.wikipedia.org/wiki/Type-length-value) sequence. The type contains information about the primitive data type of the value (e.g. string.) The length is an integer and contains the number of octets in the value.

The primitive data type is encoded in the same way as described for fixed-length types. The length type is encoded by the following bit patterns.

Bit pattern | Length type | Length in octets
--- | ---: | ---:
1010 1ZZZ | 8-bits |	1
1011 1ZZZ | 16-bits |	2
1100 1ZZZ | 32-bits |	4
1101 1ZZZ | 64-bits |	8

For example, the string "AB" (UTF-8 code 0x41 followed by 0x42) is encoded as `%xA9.02.41.42`.

The length is encoded as an [unsigned integer](http://en.wikipedia.org/wiki/Unsigned_integer). Because some programming languages do not support unsigned integers and therefore have difficulty representing an unsigned 64-bit number, we have limited the range of 64-bits lengths to be less than 2<sup>63</sup>. This means that the length can be implemented using a signed 64-bits integer. Lengths equal to or greater than 2<sup>63</sup> must be reported as errors.

```abnf
length   = length8 / length16 / length32 / length64
length8  = 1%x00-FF
length16 = 2%x00-FF
length32 = 4%x00-FF
length64 = %x00-7F.7%x00-FF
```

Unknown variable-length tokens can be skipped by advancing over the type octet, read the length type, advance over the length type, and finally advance over the subsequent number of octets determined by the length.

### Group types

Group types consists of a single octet that signifies either an opening or a closing token. They occupy the range `%x90` to `%x9F`.

Groups are used to encode composite data types. A group consists of an opening token and a corresponding closing token. An opening token must be followed by a closing token later in the stream. Groups can be nested inside other groups, so the opening and closing tokens must be balanced. Unbalanced groups must be reported as errors.

The group token uses the [least significant bit](http://en.wikipedia.org/wiki/Least_significant_bit), bit 0, to distinguish between opening and closing tokens, and bits 1-3 to identify the specific group.

Bit pattern | Group brackets
--- | ---
1001 ZZZ0 |	Open group ZZZ
1001 ZZZ1 |	Close group ZZZ

Unknown group tokens can be skipped by advancing over the opening type octet, and continue skipping over the subsequent tokens (according to their skipping rules) until the balanced closing type is encountered, and finally skipping over the closing type.

## Data Types

The abovementioned structure is used to encode the actual data types.

### Null

Null is a [nullable type](http://en.wikipedia.org/wiki/Nullable_type) that indicates the absence of a value.

```abnf
null = %x82
```

### Boolean

[Booleans](http://en.wikipedia.org/wiki/Boolean_data_type) can be either true or false.

```abnf
boolean = true / false
true    = %x81
false   = %x80
```

### Integer

Integers are [two's complement](http://en.wikipedia.org/wiki/Two%27s_complement) [signed](http://en.wikipedia.org/wiki/Signed_number_representations) numbers. The values are ordered as [little endian](http://en.wikipedia.org/wiki/Little_endian).

```abnf
int           = small-pos-int / small-neg-int / int8 / int16 / int32 / int64
small-pos-int = %x00-7F
small-neg-int = %xE0-FF
int8          = %xA0 %xZZ
int16         = %xB0 %xZZ.ZZ
int32         = %xC0 %xZZ.ZZ.ZZ.ZZ
int64         = %xD0 %xZZ.ZZ.ZZ.ZZ.ZZ.ZZ.ZZ.ZZ
```

### Floating-point

[Floats](http://en.wikipedia.org/wiki/Floating_point) are [IEEE 754](http://en.wikipedia.org/wiki/IEEE_754) encoded numbers with single or double precision. The values are ordered as [little endian](http://en.wikipedia.org/wiki/Little_endian).

```abnf
float   = float32 / float64
float32 = %xC2 %xZZ.ZZ.ZZ.ZZ
float64 = %xD2 %xZZ.ZZ.ZZ.ZZ.ZZ.ZZ.ZZ.ZZ
```

### Binary

Binary data (a.k.a. byte-buffer or octet-buffer) is a variable-length type that may contain a [BLOB](http://en.wikipedia.org/wiki/Binary_large_object).

```abnf
binary   = binary8 / binary16 / binary32 / binary64
binary8  = %xAB length8 data
binary16 = %xBB length16 data
binary32 = %xCB length32 data
binary64 = %xDB length64 data
```

### String

String is a variable-length type. The length is encoded as for binary data. The value must be [UTF-8](http://en.wikipedia.org/wiki/Utf-8) encoded. Invalid UTF-8 encoding must be reported as errors.

```abnf
string   = string8 / string16 / string32 / string64
string8  = %xA9 length8 data
string16 = %xB9 length16 data
string32 = %xC9 length32 data
string64 = %xD9 length64 data
```

## Containers

All containers are potentially nested, so opening and closing group types must be balanced.

The following rules are used to describe the specific container types.

```abnf
container = record / array / map
element   = container / null / boolean / int / float / binary / string
count     = int / null
```

### Record

A [record](http://en.wikipedia.org/wiki/Record_%28computer_science%29) (also known as tuple or struct) is a sequence of pre-determined elements. The definition of a record is assumed to be known to both the sender and the receiver, so there is no reason to embed type of size information into the encoding.

```abnf
record = open0 *element close0
open0  = %x90
close0 = %x91
```

### Array

An [array](http://en.wikipedia.org/wiki/Array_data_structure) is a sequence of elements.

```abnf
array  = open1 count *element close1
open1  = %x92
close1 = %x93
```

For a heterogenous array, the encoding uses an optional count to indicate how many elements are in the array.

For streaming arrays, the array encoding is used, but with count set to null.

### Associative array

An [associative array](http://en.wikipedia.org/wiki/Associative_array) is a collection of key-value pairs. A pair is a record with two elements.

```abnf
map    = open6 count *pair close6
pair   = open0 key value close0
key    = element
value  = element
open6  = %x9C
close6 = %x9D
```
