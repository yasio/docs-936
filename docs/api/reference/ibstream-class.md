---
title: "ibstream Class"
date: "12/10/2020"
f1_keywords: ["ibstream", "yasio/ibstream", ]
helpviewer_keywords: []
---
# ibstream_view Class

Provides the functionality of Binary Reader.

## Syntax

```cpp
namespace yasio { 
using ibstream_view = basic_ibstream_view<endian::network_convert_tag>; 
using fast_ibstream_view = basic_ibstream_view<endian::host_convert_tag>;
}
```

## Members

### Public Constructors

|Name|Description|
|----------|-----------------|
|[ibstream_view::ibstream_view](#ibstream_view)|Constructs a `ibstream_view` object.|

### Public Methods

|Name|Description|
|----------|-----------------|
|[ibstream_view::reset](#reset)|Reset input data, weak reference.|
|[ibstream_view::read](#read)|Function template, read number value.|
|[ibstream_view:read_ix](#read_ix)|Function template,read **7bit Encoded Int/Int64**.|
|[ibstream_view:read_v](#read_v)|Read blob data with **7bit Encoded Int/Int64 lenght field**.|
|[ibstream_view:read_byte](#read_byte)|Read 1 byte.|
|[ibstream_view:read_bytes](#read_bytes)|Read blob data without length field.|
|[ibstream_view::empty](#empty)|Check is stream empty.|
|[ibstream_view::data](#data)|Retrieves stream data pointer.|
|[ibstream_view::length](#length)|Retrieves size of stream.|
|[ibstream_view::seek](#seek)|Moves the read position in a stream.|

## Remarks

This class is inspired from C++17 std::string_view, it never copy any buffer during initialize and read.

## Requirements

**Header:** ibstream.hpp

## <a name="ibstream_view"></a> ibstream_view::ibstream_view

Constructs a `ibstream_view` object.

```cpp
ibstream_view();

ibstream_view(const void* data, size_t size);

ibstream_view(const obstream* obs);
```

### Parameters

*data*<br/>
The pointer to first byte of buffer.

*size*<br/>
The size of data.

*obs*<br/>
The obstream object.

### Example

TODO:

## <a name="reset"></a> ibstream_view::reset

Resets `ibstream_view` input buffer view.

```cpp
void ibstream_view::reset(const void* data, size_t size);
```

### Parameters

*data*<br/>
The pointer to first byte of buffer.

*size*<br/>
The size of data.

## <a name="read"></a> ibstream_view::read

Read number value from stream with byte order convertion.

```cpp
template<typename _Nty>
_Nty ibstream_view::read();
```

### Return Value

Returns the value to be read.

### Remarks
The type *_Nty* of value could be any (1~8bytes) integral or float types.

### Example

TODO:

## <a name="read_ix"></a> ibstream_view::read_ix

Read 7Bit Encoded Int compressed value.

```cpp
template<typename _Intty>
_Intty ibstream_view::read_ix();
```

### Return Value

Returns the value to be read.

### Remarks
The type *_Intty* of value must be one of follows

- int32_t
- int64_t

This function behavior is compatible with dotnet

- [BinaryReader.Read7BitEncodedInt()](https://docs.microsoft.com/en-us/dotnet/api/system.io.binaryreader.read7bitencodedint?view=net-5.0#System_IO_BinaryReader_Read7BitEncodedInt)
- [BinaryReader.Read7BitEncodedInt64()](https://docs.microsoft.com/en-us/dotnet/api/system.io.binaryreader.read7bitencodedint64?view=net-5.0#System_IO_BinaryReader_Read7BitEncodedInt64)

### Example

TODO:

## <a name="read_v"></a> ibstream_view::read_v

Read blob data with 7Bit Encoded Int length field.

```cpp
cxx17::string_view read_v();
```

### Return Value

Returns the blob view to be read

### Remarks
This function will read length field with 7Bit Encoded first, then call [read_bytes](#read_bytes) to read the value.

### Example

TODO:

## <a name="read_byte"></a> ibstream_view::read_byte

Read 1 byte from stream.

```cpp
uint8_t read_byte();
```

### Return Value

Returns the value to be read.

### Remarks
This function is identical to [ibstream_view::read<uint8_t>](#read)

### Example

TODO:

## <a name="read_bytes"></a> ibstream_view::read_bytes

Read byte array from stream.

```cpp
cxx17::string_view read_bytes();
```

### Return Value

The blob view to be read.

### Example

TODO:

## <a name="empty"></a> ibstream_view::empty

Tests whether the ibstream_view is empty.

```cpp
bool empty() const;
```

### Return Value

`true` if the ibstream_view empty; `false` if it has at least one byte.

### Remarks

The member function is equivalent to [length](#length) == 0.

### Example

TODO:

## <a name="data"></a> ibstream_view::data

Retrieves stream data pointer.

```cpp
const char* data() const;
```

### Return Value

A pointer to the first byte in the stream.

### Example

TODO:

## <a name="length"></a> ibstream_view::length

Returns the number of bytes in the stream.

```cpp
size_t length() const;
```

### Return Value

The current length of the stream.

### Example

TODO:

## <a name="seek"></a> ibstream_view::seek

Moves the read position in a stream.

```cpp
ptrdiff_t seek(ptrdiff_t offset, int whence);
```

### Parameters

*offset*\
An offset to move the read pointer relative to *whence*.

*whence*\
One of the `SEEK_SET`,`SEEK_CUR`,`SEEK_END` enumerations.


### Return Value

The current read poistion of the stream after seek.

### Example

TODO:

# ibstream Class

Provides the functionality of Binary Reader with buffer storage.

## Syntax

```cpp
namespace yasio { 
using ibstream = basic_ibstream<endian::network_convert_tag>; 
using fast_ibstream = basic_ibstream<endian::host_convert_tag>; 
}
```

## Members

### Public Constructors

|Name|Description|
|----------|-----------------|
|[ibstream::ibstream](#ibstream)|Constructs a `ibstream` object.|

### Public Methods

|Name|Description|
|----------|-----------------|
|[ibstream::load](#load)|Load stream from file.|

### Inheritance Hierarchy
[ibstream_view](#ibstream_view)

`ibstream`

## <a name="ibstream"></a> ibstream::ibstream

Constructs a `ibstream` object.

```cpp
ibstream(std::vector<char> blob);

ibstream(const obstream* obs);
```

### Parameters

*blob*<br/>
The input binary buffer.

*obs*<br/>
The obstream object.

### Example

TODO:

## <a name="load"></a> ibstream::load

Load the stream data from file.

```cpp
bool load(const char* filename) const;
```

### Return Value

`true` succed, `false` fail.

### Example

See: [obstream::save](obstream-class.md#save)

## See also

[obstream Class](./obstream-class.md)

[io_service Class](./io_service-class.md)
