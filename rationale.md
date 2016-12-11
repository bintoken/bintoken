# Bintoken Design Decisions

### Efficiency

The format must avoid wasting CPU and battery power on serialization and deserialization.

The format must be tokenized, as opposed to [XML](http://en.wikipedia.org/wiki/Xml) or [JSON](http://en.wikipedia.org/wiki/Json) which are textual formats that first must be tokenized by the receiver.

Processing efficiency has been prioritized over compactness, because bandwidth concerns are becoming less important.

The format is targeted at upper-level embedded devices. The majority of these devices use little endian. Therefore, the numbers are encoded in little endian rather than big endian, which is the normal network byte order for [IP](http://en.wikipedia.org/wiki/Internet_Protocol) based protocols.

### Simplicity

Subject to the other design goals, the format should be as simple as possible.

The format only supports primitive and composite data types. There is not support for references or polymorphism.

### Evolutionary Resilience

Long-living middleware will inevitably encounter devices that talk different versions of the wire protocol.

The format must be backwardscompatible, so tokens must not change meaning or encoding once they have become part of the specification.

The format must be forwardscompatible in the sense that new tokens can be handled as nullable tokens by old devices. This has two implications:
* Tokens must have a well-known structure, so that they can be skipped over. For group tokens the deserializer must skip everything from the open token to the corresponding close token, including nested groups. For other tokens, the deserializer must skip over the types and their values, so that they point to the next correct token.
* We must be able to deserialize tokens without a schema. This is opposed to format such as [D-Bus](http://en.wikipedia.org/wiki/D-Bus) or [DDS-RTSP](http://en.wikipedia.org/wiki/Real-Time_Publish-Subscribe_%28RTPS%29_Protocol), which do require a schema to be deserialized.

### Streaming

It must be possible to send elements as soon as they have been encoded, even when they are part of nested data structures. Likewise, it should be possible to decode tokens as soon as they arrive at the receiver, even when they are part of nested data structures.

[Composite data types](http://en.wikipedia.org/wiki/Composite_data_type) are not type-length-value encoded, but rather encoded with an open and close token.

### Cross-platform

The format should be portable to many platforms and programming languages.

However, the format requires the existence of octets, 64-bits signed integers, and IEEE 754 single and double precision numbers.

Unsigned integers have been omitted because they are not supported by some programming languages such as Java.

Lengths in type-length-value tokens are encoded as unsigned integers to utilize the full integer range, which keeps the format more compact and it avoids introducing invalid lengths that are negative. For example, 8-bits lengths encode the range 0 → 255. However, the maximum length is limited to 2<sup>63</sup> - 1, so lengths can fit into a signed 64-bits integer on platforms that do not support unsigned integers.
