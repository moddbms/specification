# Typed Data Exchange Format Specification

## What is TDX?

TDX, short for Typed Data Exchange, is a plain text data exchange format that supports multiple different types of data. It is the primary means of exchange between ModSQL clients and servers.

TDX supports the following types:

- ModId: **id\<T\>**, *An ID always auto-generates a value on insert unless a value is already specified*
- Guid: **gid**
- String: **str**
- Integer: **int**
- Decimal: **dec**
- Boolean: **bol**
- Hexadecimal: **hex**
- Scientific Number: **scn**
- Scripts: **scr**
- Objects: **obj**
- Dynamic: **dyn**
- DateTime: **dtm**
- TimeSpan: **tsn**
- DateOnly: **dto**
- Binary: **bin**
- Arrays: **arr\<T\>**
- Json: **jsn**
- XML: **xml**
- SQL: **sql**
- Expression: **exp\<T\>**, *Expressions are properties that are calculated on a query. It must always appear before the field(s) it references*

