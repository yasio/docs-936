---
title: "yasio::obstream Class"
date: "16/10/2020"
f1_keywords: ["obstream", "yasio/obstream", ]
helpviewer_keywords: []
---
# obstream Class

Provides the functionality of Binary Writer.

## Syntax

```cpp
namespace yasio { 
using obstream = basic_obstream<endian::network_convert_tag>;

// The fast binary writer without byte order convertion.
using fast_obstream = basic_obstream<endian::host_convert_tag>;
}
```

## Members

### Public Constructors

|Name|Description|
|----------|-----------------|
|[obstream::obstream](#obstream)|Constructs a `obstream` object.|

### Public Methods

|Name|Description|
|----------|-----------------|
|[obstream::write](#write)|Function template, write number value.|
|[obstream::write_ix](#write_ix)|Function template, write **7bit Encoded Int/Int64**.|
|[obstream::write_v](#write_v)|Write blob data with **7bit Encoded Int lenght field**.|
|[obstream::write_byte](#write_byte)|Write 1 byte.|
|[obstream::write_bytes](#write_bytes)|Write blob data without length field.|
|[obstream::empty](#empty)|Check is stream empty.|
|[obstream::data](#data)|Retrieves stream data pointer.|
|[obstream::length](#length)|Retrieves size of stream.|
|[obstream::buffer](#buffer)|Retrieves the buffer object of the stream.|
|[obstream::save](#save)|Save the stream binary data to file.|

## Remarks

When write int16~int64 and float/double, will auto convert host byte order to network byte order.

## Requirements

**Header:** obstream.hpp

## <a name="obstream"></a> obstream::obstream

Constructs a `obstream` object.

```cpp
obstream(size_t capacity = 128);

obstream(const obstream& rhs);

obstream(obstream&& rhs);
```

### Example

TODO:

## <a name="write"></a> obstream::write

Write number value to stream with byte order convertion.

```cpp
template<typename _Nty>
void obstream::write(_Nty value);
```

### Parameters

*value*<br/>
The value to be written.

### Remarks
The type *_Nty* of value could be any (1~8bytes) integral or float types 

### Example

TODO:

## <a name="write_ix"></a> obstream::write_ix

Write 7Bit Encoded Int compressed value.

```cpp
template<typename _Intty>
void obstream::write_ix(_Intty value);
```

### Parameters

*value*<br/>
The value to be written.

### Remarks
The type *_Intty* of value must be one of follows

- int32_t
- int64_t

This function behavior is compatible with dotnet

- [BinaryWriter.Write7BitEncodedInt](https://docs.microsoft.com/en-us/dotnet/api/system.io.binarywriter.write7bitencodedint?view=net-5.0#System_IO_BinaryWriter_Write7BitEncodedInt_System_Int32_)
- [BinaryWriter.Write7BitEncodedInt64](https://docs.microsoft.com/en-us/dotnet/api/system.io.binarywriter.write7bitencodedint64?view=net-5.0#System_IO_BinaryWriter_Write7BitEncodedInt64_System_Int64_)

### Example

TODO:

## <a name="write_v"></a> obstream::write_v

Write blob data with 7Bit Encoded Int length field.

```cpp
void write_v(cxx17::string_view sv);
```

### Parameters

*sv*<br/>
The string_view value to be written.

### Remarks
This function will write length field with 7Bit Encoded first, then call [write_bytes](#write_bytes) to write the value.

### Example

TODO:

## <a name="write_byte"></a> obstream::write_byte

Write 1 byte to stream.

```cpp
void write_byte(uint8_t value);
```

### Parameters

*value*<br/>
The value to be written.

### Remarks
This function is identical to [obstream::write<uint8_t>](#write)

### Example

TODO:

## <a name="write_bytes"></a> obstream::write_bytes

Write byte array to stream.

```cpp
void write_bytes(cxx17::string_view sv);

void write_bytes(const void* data, int length);

void write_bytes(std::streamoff offset, const void* data, int length);
```

### Parameters

*sv*<br/>
The string_view value to be written.

*data*<br/>
The data to be written.

*length*<br/>
The length data to be written.

*offset*<br/>
The offset of stream to be written.

### Remarks
The value of `offset + length` must be less than [`obstream::length`](#length)

### Example

TODO:

## <a name="empty"></a> obstream::empty

Tests whether the obstream is empty.

```cpp
bool empty() const;
```

### Return Value

`true` if the obstream empty; `false` if it has at least one byte.

### Remarks

The member function is equivalent to [length](#length) == 0.

### Example

TODO:

## <a name="data"></a> obstream::data

Retrieves stream data pointer.

```cpp
const char* data() const;

char* data();
```

### Return Value

A pointer to the first byte in the stream.

### Example

TODO:

## <a name="length"></a> obstream::length

Returns the number of bytes in the stream.

```cpp
size_t length() const;
```

### Return Value

The current length of the stream.

### Example

TODO:

## <a name="buffer"></a> obstream::buffer

Retrieves internal buffer of stream.

```cpp
const std::vector<char>& buffer() const;

std::vector<char>& buffer();
```

### Return Value

The internal implementation buffer of the stream.

### Example

```cpp
// obstream_buffer.cpp
// compile with: /EHsc
#include "yasio/obstream.hpp"

int main( )
{
   using namespace yasio;
   using namespace cxx17;
   
   obstream obs;
   obs.write_v("hello world!");
   
   const auto& const_buffer = obs.buffer();

   // after this line, the obs will be empty
   auto move_buffer = std::move(obs.buffer());

   return 0;
}
```

## <a name="save"></a> obstream::save

Save the stream data to file.

```cpp
void save(const char* filename) const;
```

### Example

```cpp
// obstream_save.cpp
// compile with: /EHsc
#include "yasio/obstream.hpp"
#include "yasio/ibstream.hpp"

int main( )
{
   using namespace yasio;
   using namespace cxx17;
   
   obstream obs;
   obs.write_v("hello world!");
   obs.save("obstream_save.bin");

   ibstream ibs;
   if(ibs.load("obstream_save.bin")) {
       // output should be: hello world!
       try {
           std::count << ibs.read_v() << "\n";
       }
       catch(const std::exception& ex) {
           std::count << "read_v fail: " <<
               << ex.message() << "\n";
       }
   }

   return 0;
}
```

## See also

[ibstream_view Class](./ibstream-class.md)

[ibstream Class](./ibstream-class.md#ibstream-class)
