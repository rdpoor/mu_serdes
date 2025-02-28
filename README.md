# mu_serdes
Convert between internal and external representation of objects via
serialization and deserialization.  Provides APIs for C and Python.

Provides efficient coding of several immediate types (unsigned 7-bit ints, true,
false, NULL),
## supported types

In the serialized form, each supported type can be identified by the first byte
in the byte stream, and each object has a defined length.

In the table below, `<ali>` represents an Arbitrary Length Integer -- see below
for more information.

+---+---+---+---+---+---+
| header | serialized form |c type| python type | range | comment |
+---+---+---+---+---+---+
| 0xxxxxxx | immediate | uint8 | int | 0..127 | immediate unsigned int |
| 10000000 | immediate | NULL | None | immediate |  |
| 10000001 | immediate | true | True | immediate |  |
| 10000010 | immediate | false | False | immediate |  |
| 10000011 | `<b0>` | int8 | int | -2^8..2^8-1 | signed byte |
| 10000100 | `<b0>` | uint8 | int | 0..255 | unsigned byte |
| 10000101 | `<b0..b1>` | int16 | int | -2^16..2^16-1 | signed 16 bit |
| 10000110 | `<b0..b1>` | uint16 | int | 0..2^16-1 | unsigned 16 bit |
| 10000101 | `<b0..b3>` | int32 | int | -2^32..2^32-1 | signed 32 bit |
| 10000110 | `<b0..b3>` | uint32 | int | 0..0..2^32-1 | unsigned 32 bit |
| 10000101 | `<b0..b7>` | int64 | int | -2^63..2^63-1 | signed 32 bit |
| 10000110 | `<b0..b7>` | uint64 | int | 0..2^64-1 | unsigned 32 bit |
| 10000111 | `<b0..b3>` | float | float | -FLT_MAX..FLT_MAX | float |
| 10001000 | `<b0..b7>` | double | float | -DBL_MAX..DBL_MAX | double |
| 10001001 | `<ali> <b0>..\0` | char * | str | | null-terminated latin-8 string |
| 10001010 | `<ali> <b0>..<bN-1>` | unsigned char * | bytes | | unsigned byte array |
| 10001011 | `<ali> <type> <obj0>..<objN-1>` | `<type>*` | array | | homogeneous array |
| 10001100 | `<ali> <typed_obj0>..<typed_objN-1>` | no representation | array || heterogenous array |

