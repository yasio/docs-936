---
title: "yasio::inet::io_event Class"
date: "1/5/2021"
f1_keywords: ["io_event", "yasio/io_event", ]
helpviewer_keywords: []
---

# io_event Class

The event produced by io_service thread.


## Syntax

```cpp
namespace yasio { namespace inet { class io_event; } }
```

### Public Methods

|Name|Description|
|----------|-----------------|
|[io_event::kind](#kind)|Gets kind of event.|
|[io_event::status](#status)|Gets status of event.|
|[io_event::packet](#packet)|Gets packet of event.|
|[io_event::timestamp](#timestamp)|Gets timestamp of event.|
|[io_event::transport](#transport)|Gets transport of event.|
|[io_event::transport_id](#transport_id)|Gets transport id of event.|
|[io_event::transport_udata](#transport_udata)|Gets/Sets transport user data.|

.. _kind:

## <a name="kind"></a> io_event::kind

Gets kind of event.

```cpp
int kind() const;
```

### Return value

Retrun the kind value, can be follow values

* `YEK_PACKET`: Packet event
* `YEK_CONNECT_RESPONSE`: Connect response event
* `YEK_CONNECTION_LOST`: Connection lost event

## <a name="status"></a> io_event::status

Gets the status of event.

```cpp
int status() const;
```

### Return Value

- 0: No error
- NZ: error occured, user only needs print the error status code.

## <a name="packet"></a> io_event::packet

Gets packet of event.

```cpp
std::vector<char>& packet()
```

## Retrun value

Retrun the mutable reference to packet of event, user can use std::move to move it.

## <a name="timestamp"></a> io_event::timestamp

Get timestamp in microseconds of event.

```cpp
highp_time_t timestamp() const;
```

### Return value

Return the timestamp in macroseconds.

## <a name="transport_id"></a> io_event::transport_id

Gets transport unique id.

```cpp
unsigned int transport_id() const;
```

### Return Value

Return a unique id range in 32 bit uint.

## <a name="transport_udata"></a> io_event::transport_udata

Sets or Gets transport userdata.

```cpp
template<typename _Uty>
_Uty io_event::transport_udata();

template<typename _Uty>
void io_event::transport_udata(_Uty uservalue);
```

### Remark

User should manage the gc of userdata, such as:  

* Store userdata when receive connect success event.
* Cleanup the userdata when receive connection lost.

## See also

[io_service Class](./io_service-class.md)

[io_channel Class](./io_channel-class.md)
