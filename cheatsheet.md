# Bintoken Cheat Sheet

This table contains a summary of all the currently defined tokens. Any token not mentioned below is reserved for future use. Ellipsis (`...`) indicates a continuous range of tokens beween the row above and the row below.

Hex |	Binary | Type | Size | Description
:---: | --- | --- | ---: | ---
`%x00` | 0000 0000 | `int8` | 1 | Value 0.
`...` | | | |
`%x7F` | 0111 1111 | `int8` | 1 | Value 127.
`%x80` | 1000 0000 | `bool` | 1 | False.
`%x81` | 1000 0001 | `bool` | 1 | True.
`%x82` | 1000 0010 | `null` | 1 | Null.
`%x90` | 1001 0000 | `open0` | 1 | Open record.
`%x91` | 1001 1000 | `close0` | 1 | Close record.
`%x92` | 1001 0001 | `open1` | 1 | Open array.
`%x93` | 1001 1111 | `close1` | 1 | Close array.
`%x9C` | 1001 0010 | `open6` | 1 | Open associative array.
`%x9D` | 1001 1111 | `close6` | 1 | Close associative array.
`%xA0` | 1010 0000 | `int8` | 1 + 1 | 8-bits signed integer.
`%xA9` | 1010 1001 | `string8` | 1 + 1 + N | String with int8 length.<br/>N = number of octets in string.
`%xAB` | 1010 1000 | `binary8` | 1 + 1 + N | Octet array with int8 length.<br/>N = Number of octets in array.
`%xB0` | 1011 0000 | `int16` | 1 + 2| 16-bits signed integer.
`%xB9` | 1011 1001 | `string16` | 1 + 2 + N | String with int16 length.<br/>N = number of octets in array.
`%xBB` | 1011 1000 | `binary16` | 1 + 2 + N | Octet array with int16 length.<br/>N = Number of octets in array.
`%xC0` | 1100 0000 | `int32` | 1 + 4 | 32-bits signed integer.
`%xC2` | 1100 0010 | `float32` | 1 + 4 | 32-bits float.
`%xC9` | 1100 1001 | `string32` | 1 + 4 + N | String with int32 length.<br/>N = Number of octets in string.
`%xCB` | 1100 1000 | `binary32` | 1 + 4 + N | Octet array with int32 length.<br/>N = Number of octets in array.
`%xD0` | 1101 0000 | `int64` | 1 + 8 | 64-bits signed integer.
`%xD2` | 1101 0010 | `float64` | 1 + 8 | 64-bits float.
`%xD9` | 1101 1001 | `string64` | 1 + 8 + N | String with int64 length.<br/>N = Number of octets in string.
`%xDB` | 1101 1000 | `binary64` | 1 + 8 + N | Octet array with int64 length.<br/>N = Number of octets in array.
`%xE0` | 1110 0000 | `int8` | 1 | Value -32.
`...` | | | |
`%xFF` | 1111 1111 | `int8` | 1 | Value -1.
