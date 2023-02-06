# Serialization specifications for the binary TDX data format

### General serialization structure

The TDX object is represented in the following way:

```
[192, 88] // start of document
[property count (int32)]

```