msgpack-spec
============
Type Chart
----------
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
