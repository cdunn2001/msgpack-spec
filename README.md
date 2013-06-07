(Copied from [here](http://wiki.msgpack.org/display/MSGPACK/Format+specification#).)

This page describes the MessagePack data format, which is required for developing the MessagePack language bindings.

# MessagePack format specification

MessagePack saves type-information to the serialized data. Thus each data is stored in *type-data* or *type-length-data* style.
MessagePack supports following types:

* Fixed length types
 * Integers
 * nil
 * boolean
 * Floating point
* Variable length types
 * Raw bytes
* Container types
 * Arrays
 * Maps
Each type has one or more serialize format:
* Fixed length types
 * Integers
  * positive fixnum
  * negative fixnum
  * uint 8
  * uint 16
  * uint 32
  * uint 64
  * int 8
  * int 16
  * int 32
  * int 64
 * Nil
  * nil
 * Boolean
  * true
  * false
 * Floating point
  * float
  * double
* Variable length types
 * Raw bytes
  * fix raw
  * raw 16
  * raw 32
* Container types
 * Arrays
  * fix array
  * array 16
  * array 32
 * Maps
  * fix map
  * map 16
  * map 32

To serialize strings, use UTF-8 encoding and Raw type.

See [this thread](https://github.com/msgpack/msgpack/issues/121) to understand the reason why msgpack doesn't have string type.

## Integers

### positive fixnum
save an integer within the range 0, 127 in 1 bytes.
```
+--------+
|0XXXXXXX|
+--------+
=> unsigned 8-bit 0XXXXXXX
```
### negative fixnum
save an integer within the range -32, -1 in 1 bytes.
```
+--------+
|111XXXXX|
+--------+
=> signed 8-bit 111XXXXX
```
### uint 8
save an unsigned 8-bit integer in 2 bytes.
```
+--------+--------+
|  0xcc  |XXXXXXXX|
+--------+--------+
=> unsigned 8-bit XXXXXXXX
```
### uint 16
save an unsigned 16-bit big-endian integer in 3 bytes.
```
+--------+--------+--------+
|  0xcd  |XXXXXXXX|XXXXXXXX|
+--------+--------+--------+
=> unsigned 16-bit big-endian XXXXXXXX_XXXXXXXX
```
### uint 32
save an unsigned 32-bit big-endian integer in 5 bytes.
```
+--------+--------+--------+--------+--------+
|  0xce  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|
+--------+--------+--------+--------+--------+
=> unsigned 32-bit big-endian XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX
```
### uint 64
save an unsigned 64-bit big-endian integer in 9 bytes.
```
+--------+--------+--------+--------+--------+--------+--------+--------+--------+
|  0xcf  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|
+--------+--------+--------+--------+--------+--------+--------+--------+--------+
=> unsigned 64-bit big-endian XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX
```
### int 8
save a signed 8-bit integer in 2 bytes.
```
+--------+--------+
|  0xd0  |XXXXXXXX|
+--------+--------+
=> signed 8-bit XXXXXXXX
```
### int 16
save a signed 16-bit big-endian integer in 3 bytes.
```
+--------+--------+--------+
|  0xd1  |XXXXXXXX|XXXXXXXX|
+--------+--------+--------+
=> signed 16-bit big-endian XXXXXXXX_XXXXXXXX
```
### int 32
save a signed 32-bit big-endian integer in 5 bytes.
```
+--------+--------+--------+--------+--------+
|  0xd2  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|
+--------+--------+--------+--------+--------+
=> signed 32-bit big-endian XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX
```
### int 64
save a signed 64-bit big-endian integer in 9 bytes.
```
+--------+--------+--------+--------+--------+--------+--------+--------+--------+
|  0xd3  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|
+--------+--------+--------+--------+--------+--------+--------+--------+--------+
=> signed 64-bit big-endian XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX
```
## Nil
### nil
save a nil.
```
+--------+
|  0xc0  |
+--------+
```
## Boolean
### true
save a true.
```
+--------+
|  0xc3  |
+--------+
```
### false
save a false.
```
+--------+
|  0xc2  |
+--------+
```
## Floating point
### float
save a big-endian IEEE 754 single precision floating point number in 5 bytes.
```
+--------+--------+--------+--------+--------+
|  0xca  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|
+--------+--------+--------+--------+--------+
=> big-endian IEEE 754 single precision floating point number XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX
```
### double
save a big-endian IEEE 754 double precision floating point number in 9 bytes.
```
+--------+--------+--------+--------+--------+--------+--------+--------+--------+
|  0xcb  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|
+--------+--------+--------+--------+--------+--------+--------+--------+--------+
=> big-endian IEEE 754 single precision floating point number XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX
```
## Raw bytes
### fix raw
save raw bytes up to 31 bytes.
```
+--------+--------
|101XXXXX|...N bytes
+--------+--------
=> 000XXXXXX (=N) bytes of raw bytes.
```
### raw 16
save raw bytes up to (2^16)-1 bytes. Length is stored in unsigned 16-bit big-endian integer.
```
+--------+--------+--------+--------
|  0xda  |XXXXXXXX|XXXXXXXX|...N bytes
+--------+--------+--------+--------
=> XXXXXXXX_XXXXXXXX (=N) bytes of raw bytes.
```
### raw 32
save raw bytes up to (2^32)-1 bytes. Length is stored in unsigned 32-bit big-endian integer.
```
+--------+--------+--------+--------+--------+--------
|  0xdb  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|...N bytes
+--------+--------+--------+--------+--------+--------
=> XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX (=N) bytes of raw bytes.
```
## Arrays
### fix array
save an array up to 15 elements.
```
+--------+--------
|1001XXXX|...N objects
+--------+--------
=> 0000XXXX (=N) elements array.
```
### array 16
save an array up to (2^16)-1 elements. Number of elements is stored in unsigned 16-bit big-endian integer.
```
+--------+--------+--------+--------
|  0xdc  |XXXXXXXX|XXXXXXXX|...N objects
+--------+--------+--------+--------
=> XXXXXXXX_XXXXXXXX (=N) elements array.
```
### array 32
save an array up to (2^32)-1 elements. Number of elements is stored in unsigned 32-bit big-endian integer.
```
+--------+--------+--------+--------+--------+--------
|  0xdd  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|...N objects
+--------+--------+--------+--------+--------+--------
=> XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX (=N) elements array.
```
## Maps
### fix map
save a map up to 15 elements.
```
+--------+--------
|1000XXXX|...N*2 objects
+--------+--------
=> 0000XXXX (=N) elements map
```
   where odd elements are key and next element of the key is its associate value.
### map 16
save a map up to (2^16)-1 elements. Number of elements is stored in unsigned 16-bit big-endian integer.
```
+--------+--------+--------+--------
|  0xde  |XXXXXXXX|XXXXXXXX|...N*2 objects
+--------+--------+--------+--------
=> XXXXXXXX_XXXXXXXX (=N) elements map
   where odd elements are key and next element of the key is its associate value.
```
### map 32
save a map up to (2^32)-1 elements. Number of elements is stored in unsigned 32-bit big-endian integer.
```
+--------+--------+--------+--------+--------+--------
|  0xdf  |XXXXXXXX|XXXXXXXX|XXXXXXXX|XXXXXXXX|...N*2 objects
+--------+--------+--------+--------+--------+--------
=> XXXXXXXX_XXXXXXXX_XXXXXXXX_XXXXXXXX (=N) elements map
   where odd elements are key and next element of the key is its associate value.
```
# Type Chart
```
Type      Binary   Hex
FixNumPos 0xxxxxxx 0x00 - 0x7f
FixMap    1000xxxx 0x80 - 0x8f
FixArray  1001xxxx 0x90 - 0x9f
FixRaw    101xxxxx 0xa0 - 0xbf
nil       11000000 0xc0
reserved  11000001 0xc1
false     11000010 0xc2
true      11000011 0xc3
reserved  11000100 0xc4
reserved  11000101 0xc5
reserved  11000110 0xc6
reserved  11000111 0xc7
reserved  11001000 0xc8
reserved  11001001 0xc9
float     11001010 0xca
double    11001011 0xcb
uint8     11001100 0xcc
uint16    11001101 0xcd
uint32    11001110 0xce
uint64    11001111 0xcf
int8      11010000 0xd0
int16     11010001 0xd1
int32     11010010 0xd2
int64     11010011 0xd3
reserved  11010100 0xd4
reserved  11010101 0xd5
reserved  11010110 0xd6
reserved  11010111 0xd7
reserved  11011000 0xd8
reserved  11011001 0xd9
raw16     11011010 0xda
raw32     11011011 0xdb
array16   11011100 0xdc
array32   11011101 0xdd
map16     11011110 0xde
map32     11011111 0xdf
FixNumNeg 111xxxxx 0xe0 - 0xff
```
