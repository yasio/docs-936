---
title: "yasio::inet::io_channel Class"
date: "1/5/2021"
f1_keywords: ["io_channel", "yasio/io_channel", ]
helpviewer_keywords: []
---

# io_channel Class

Provides the functionality of establishing tcp/udp/kcp connections.


## Syntax

```cpp
namespace yasio { namespace inet { class io_channel; } }
```

### Public Methods

|Name|Description|
|----------|-----------------|
|[io_channel::get_service](#get_service)|Gets belong service of channel.|
|[io_channel::index](#index)|Gets index of channel at service.|
|[io_channel::remote_port](#remote_port)|Gets remote port of channel.|

## Remarks

Once io_service initialized, the max count of channel can't be changed. <br/>
Retrieves through io_service::cindex_to_handle.


## <a name="get_service"></a> io_channel::get_service

Gets owner service.

```cpp
io_service& get_service()
```

## <a name="index"></a> io_channel::index

Gets channel index at service.

```cpp
int index() const
```

## <a name="remote_port"></a> io_channel::remote_port

Gets remote port.

```cpp
u_short remote_port() const;
```

### Return value

Return remote port of channel

- For client channel, it's port to connect.  
- For server channel, it's port to listen.

## See also

[io_service Class](./io_service-class.md)

[io_event Class](./io_event-class.md)
