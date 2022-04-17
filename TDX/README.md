# Typed Data Exchange format specification

## What is TDX

TDX, or short for Typed Data Exchange is a plain text data exchange format that supports multiple different types of data, it is the primary means of exchange between ModSQL clients and servers.

TDX supports the following types:

- ModId: **id\<T\>**, *An ID always auto=generates a value on insert unless a value is already specified*
- Guid: **gi**
- String: **st**
- Integer: **in**
- Decimal: **dc**
- Boolean: **bo**
- Hexadecimal: **hx**
- Scientific Number: **sn**
- Scripts: **sc**
- Objects: **ob**
- Dynamic: **dy**
- DateTime: **dt**
- DateOnly: **do**
- Binary: **bi**
- Arrays: **ar\<T\>**
- Objects: **ob**
- Json: **jn**
- XML: **xm**
- Expression: **ex\<T\>**, *Expressions are properties that are calculated on query, it must always appear before the fields it references*

