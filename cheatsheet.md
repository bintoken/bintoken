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
`%xA0` | 1010 0000 | `int8` | 1 + 1 | 8-bits integer.
`%xA8` | 1010 1000 | `array8_int8` | 1 + 1 + N | 8-bits integer array with 8-bits length.<br/>N = Number of octets in array.
`%xA9` | 1010 1001 | `string8` | 1 + 1 + N | String with 8-bits length.<br/>N = number of octets in string.
`%xAA` | 1010 1010 | `array8_int16` | 1 + 1 + N | 16-bits integer array with 8-bits length.<br/>N = Number of octets in array.
`%xAC` | 1010 1100 | `array8_int32` | 1 + 1 + N | 32-bits integer array with 8-bits length.<br/>N = Number of octets in array.
`%xAD` | 1010 1101 | `array8_float32` | 1 + 1 + N | 32-bits float array with 8-bits length.<br/>N = Number of octets in array.
`%xAE` | 1010 1110 | `array8_int64` | 1 + 1 + N | 64-bits integer array with 8-bits length.<br/>N = Number of octets in array.
`%xAF` | 1010 1111 | `array8_float64` | 1 + 1 + N | 64-bits float array with 8-bits length.<br/>N = Number of octets in array.
`%xB2` | 1011 0010 | `int16` | 1 + 2| 16-bits integer.
`%xB8` | 1011 1000 | `array16_int8` | 1 + 2 + N | 8-bits integer array with 16-bits length.<br/>N = Number of octets in array.
`%xB9` | 1011 1001 | `string16` | 1 + 2 + N | String with 16-bits length.<br/>N = number of octets in array.
`%xBA` | 1011 1010 | `array16_int16` | 1 + 2 + N | 16-bits integer array with 16-bits length.<br/>N = Number of octets in array.
`%xBC` | 1011 1100 | `array16_int32` | 1 + 2 + N | 32-bits integer array with 16-bits length.<br/>N = Number of octets in array.
`%xBD` | 1011 1101 | `array16_float32` | 1 + 2 + N | 32-bits float array with 16-bits length.<br/>N = Number of octets in array.
`%xBE` | 1011 1110 | `array16_int64` | 1 + 2 + N | 64-bits integer array with 16-bits length.<br/>N = Number of octets in array.
`%xBF` | 1011 1111 | `array16_float64` | 1 + 2 + N | 64-bits float array with 16-bits length.<br/>N = Number of octets in array.
`%xC4` | 1100 0100 | `int32` | 1 + 4 | 32-bits integer.
`%xC5` | 1100 0110 | `float32` | 1 + 4 | 32-bits float.
`%xC8` | 1100 1000 | `array32_int8` | 1 + 4 + N | 8-bits integer array with 32-bits length.<br/>N = Number of octets in array.
`%xC9` | 1100 1001 | `string32` | 1 + 4 + N | String with 32-bits length.<br/>N = Number of octets in string.
`%xCA` | 1100 1010 | `array32_int16` | 1 + 4 + N | 16-bits integer array with 32-bits length.<br/>N = Number of octets in array.
`%xCC` | 1100 1100 | `array32_int32` | 1 + 4 + N | 32-bits integer array with 32-bits length.<br/>N = Number of octets in array.
`%xCD` | 1100 1101 | `array32_float32` | 1 + 4 + N | 32-bits float array with 32-bits length.<br/>N = Number of octets in array.
`%xCE` | 1100 1110 | `array32_int64` | 1 + 4 + N | 64-bits integer array with 32-bits length.<br/>N = Number of octets in array.
`%xCF` | 1100 1111 | `array32_float64` | 1 + 4 + N | 64-bits float array with 32-bits length.<br/>N = Number of octets in array.
`%xD6` | 1101 0110 | `int64` | 1 + 8 | 64-bits integer.
`%xD7` | 1101 0111 | `float64` | 1 + 8 | 64-bits float.
`%xD8` | 1101 1000 | `array64_int8` | 1 + 8 + N | 8-bit integer array with 64-bits length.<br/>N = Number of octets in array.
`%xD9` | 1101 1001 | `string64` | 1 + 8 + N | String with 64-bits length.<br/>N = Number of octets in string.
`%xDA` | 1101 1010 | `array64_int16` | 1 + 8 + N | 16-bits integer array with 64-bits length.<br/>N = Number of octets in array.
`%xDC` | 1101 1100 | `array64_int32` | 1 + 8 + N | 32-bits integer array with 64-bits length.<br/>N = Number of octets in array.
`%xDD` | 1101 1101 | `array64_float32` | 1 + 8 + N | 32-bits float array with 64-bits length.<br/>N = Number of octets in array.
`%xDE` | 1101 1110 | `array64_int64` | 1 + 8 + N | 64-bits integer array with 64-bits length.<br/>N = Number of octets in array.
`%xDF` | 1101 1111 | `array64_float64` | 1 + 8 + N | 64-bits float array with 64-bits length.<br/>N = Number of octets in array.
`%xE0` | 1110 0000 | `int8` | 1 | Value -32.
`...` | | | |
`%xFF` | 1111 1111 | `int8` | 1 | Value -1.
