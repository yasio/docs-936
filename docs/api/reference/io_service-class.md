---
title: "yasio::inet::io_service Class"
date: "12/11/2020"
f1_keywords: ["io_service", "yasio/io_service", ]
helpviewer_keywords: []
---
# io_service Class

Provides the functionality of `tcp, udp, kcp and ssl-client`  communication with noblocking-io model.

## Syntax

```cpp
namespace yasio { namespace inet { class io_service; } }
```

## Members

### Public Constructors

|Name|Description|
|----------|-----------------|
|[io_service::io_service](#io_service)|Constructs a `io_service` object.|

### Public Methods

|Name|Description|
|----------|-----------------|
|[io_service::start](#start)|Start the network service thread.|
|[io_service::stop](#stop)|Stop the network service thread.|
|[io_service::open](#open)|Open channel.|
|[io_service::close](#close)|Close transport.|
|[io_service::is_open](#is_open)|Tests whether channel or transport is open.|
|[io_service::dispatch](#dispatch)|Dispatch the network io events.|
|[io_service::write](#write)|Sends data asynchronous.|
|[io_service::write_to](#write_to)|Sends data to specific remote asynchronous.|
|[io_service::schedule](#schedule)|Save the stream binary data to file.|
|[io_service::init_globals](#init_globals)|Init global data with print function callback.|
|[io_service::cleanup_globals](#cleanup_globals)|Cleanup the global print function callback.|
|[io_service::channel_at](#channel_at)|Retrieves the channel by index.|
|[io_service::set_option](#set_option)|Set options.|

## Remarks

By default, the transport use object_pool.

## Requirements

**Header:** yasio.hpp

## <a name="io_service"></a> io_service::io_service

Constructs a `io_service` object.

```cpp
io_service::io_service();

io_service::io_service(int channel_count);

io_service::io_service(const io_hostent& channel_ep);

io_service::io_service(const io_hostent* channel_eps, int channel_count);
```

### Parameters

*channel_count*<br/>
The channel count.

*channel_ep*<br/>
The channel endpoint.

*channel_eps*<br/>
The first pointer of channel endpoints.

### Example

```cpp
#include "yasio/yasio.hpp"
int main() {
    using namespace yasio;
    using namespace yasio::inet;
    io_service s1; // s1 only support 1 channel
    io_service s2(5); // s2 support 5 channels concurrency
    io_service s3(io_hostent{"github.com", 443}); // s3 support 1 channel
    io_hostent hosts[] = {  
        {"192.168.1.66", 20336},
        {"192.168.1.88", 20337},
    };
    io_service s4(hosts, YASIO_ARRAYSIZE(hosts)); // s4 support 2 channels concurrency
    return 0;
}
```

## <a name="start"></a> io_service::start

Start the network service thread.

```cpp
void start(io_event_cb_t cb);
```

### Parameters

*cb*<br/>
The callback to receive network io events.

### Example

```cpp
#include "yasio/yasio.hpp"
int main() {
    using namespace yasio;
    using namespace yasio::inet;
    auto service = yasio_shared_service(io_hostent{host="ip138.com", port=80});
    service->start([](event_ptr&& ev) {
    auto kind = ev->kind();
    if (kind == YEK_CONNECT_RESPONSE)
    {
        if (ev->status() == 0)
        printf("[%d] connect succeed.\n", ev->cindex());
        else
        printf("[%d] connect failed!\n", ev->cindex());
    }
    });
    return 0;
}
```

## <a name="stop"></a> io_service::stop

Stop network service thread.

```cpp
void stop()
```

### Remarks
If the network service thread running, this function will post exit signal and wait it exit properly.

### Example

TODO:

## <a name="open"></a> io_service::open

Open a channel.

```cpp
void open(size_t cindex, int kind);
```

### Parameters

*cindex*<br/>
The index of channel.

*kind*<br/>
The kind of channel.

### Remarks

For tcp, will start a non-blocking 3 times handshake to establish tcp connection.

The *cindex* value must be less than max channels supported by this io_service.

The *kind* must be follow values

- `YCK_TCP_CLIENT`
- `YCK_TCP_SERVER`
- `YCK_UDP_CLIENT`
- `YCK_UDP_SERVER`
- `YCK_KCP_CLIENT`
- `YCK_KCP_SERVER`
- `YCK_SSL_CLIENT`

### Example

TODO:

## <a name="close"></a> io_service::close

Close the channel or transport.

```cpp
void close(transport_handle_t transport);

void close(int cindex);
```

### Parameters

*transport*<br/>
The transport to be close.

*cindex*<br/>
The channel index to be close.

### Remarks
For tcp, will trigger 4 times handsake to terminate the connection.

### Example

TODO:

## <a name="is_open"></a> io_service::is_open

Tests whether the transport or channel is open.

```cpp
bool is_open(transport_handle_t transport) const;

bool is_open(int cindex) const;
```

### Parameters

*transport*<br/>
The transport to be tests.

*cindex*<br/>
The index of channel to be tests.

### Example

TODO:

## <a name="dispatch"></a> io_service::dispatch

Consume network events queue and dispatch them.

```cpp
void dispatch(int max_count);
```

### Parameters

*max_count*<br/>
The max count allow to dispatch at this time.

### Remarks

Usually, this function should call at logic thread, such as cocos2d-x render thread or other game engine main thread.

It's useful to update game ui safety.

### Example

```cpp
yasio_shared_service()->dispatch(128);
```

## <a name="write"></a> io_service::write

Sends data asynchronous.

```cpp
int write(
    transport_handle_t thandle,
    std::vector<char> buffer,
    io_completion_cb_t completion_handler = nullptr
);
```

### Parameters

*thandle*<br/>
The transport handle to send.

*buffer*<br/>
The send buffer.

*completion_handler*<br/>
The completion handler for send operation.

### Return Value

A number of bytes to sends, error occured when < 0.

### Remarks

The *completion_handler* not support KCP.

The empty buffer will be ignored and not trigger completion_handler.

### Example

TODO:

## <a name="write_to"></a> io_service::write_to

Sends data asynchronous.

```cpp
int write_to(
    transport_handle_t thandle,
    std::vector<char> buffer,
    const ip::endpoint& to,
    io_completion_cb_t completion_handler = nullptr
);
```

### Parameters

*thandle*<br/>
The transport handle to send.

*buffer*<br/>
The send buffer.

*to*<br/>
The remote endpoint for send operation.

*completion_handler*<br/>
The completion handler for send operation.

### Return Value

A number of bytes to be send, error occured when < 0.

### Remarks

This function only works for *DGRAM* transport `udp,kcp`

The *completion_handler* not support KCP.

The empty buffer will be ignored and not trigger completion_handler.

### Example

TODO:

## <a name="schedule"></a> io_service::schedule

Schedule a timer which will dispatch on the network service thread.

```cpp
highp_timer_ptr schedule(
    const std::chrono::microseconds& duration,
    timer_cb_t cb
);
```

### Parameters

*duration*<br/>
The timer expire duration.

*cb*<br/>
The callback to execute when the timer is expired.

### Return Value

The shared_ptr of the high resolution timer.

### Example

```cpp
// Register a once timer, timeout is 3 seconds.
yasio_shared_service()->schedule(std::chrono::seconds(3), []()->bool{
  printf("time called!\n");
  return true;
});

// Register a loop timer, interval is 5 seconds.
auto loopTimer = yasio_shared_service()->schedule(std::chrono::seconds(5), []()->bool{
  printf("time called!\n");
  return false;
});
```

## <a name="init_globals"></a> io_service::init_globals

Explicit init global data with print function callback.

```cpp
static void init_globals(print_fn2_t print_fn);
```

### Parameters

*print_fn*<br/>
The custom print function to print network service log.

### Remarks

This function is optional, it's useful to redirect network service log to your custom log system, such as ue4,u3d, see the example.

### Example

```cpp
// yasio_uelua.cpp
// compile with: /EHsc
#include "yasio_uelua.h"
#include "yasio/platform/yasio_ue4.hpp"
#include "lua.hpp"
#if defined(NS_SLUA)
using namespace NS_SLUA;
#endif
#include "yasio/bindings/lyasio.cpp"

DECLARE_LOG_CATEGORY_EXTERN(yasio_ue4, Log, All);
DEFINE_LOG_CATEGORY(yasio_ue4);

void yasio_uelua_init(void* L)
{
  auto Ls            = (lua_State*)L;
  print_fn2_t log_cb = [](int level, const char* msg) {
    FString text(msg);
    const TCHAR* tstr = *text;
    UE_LOG(yasio_ue4, Log, L"%s", tstr);
  };
  io_service::init_globals(log_cb);

  luaregister_yasio(Ls);
}
void yasio_uelua_cleanup()
{
  io_service::cleanup_globals();
}
```

## <a name="cleanup_globals"></a> io_service::cleanup_globals

Clear custom print function object.

```cpp
static void cleanup_globals();
```

### Remarks

You should call this function before unload a module(.dll,.so) which contains custom print function object.

## <a name="channel_at"></a> io_service::channel_at

Retrieves channel by index.

```cpp
io_channel* channel_at(size_t cindex) const;
```

### Parameters

*cindex*<br/>
The index of channel.

### Return value

The channel pointer, will be `nullptr` if the index out-of-range.

## <a name="set_option"></a> io_service::set_option

Set current io_service option.

```cpp
void set_option(int opt, ...);
```

### Parameters

*opt*<br/>
The opt value, see [YOPT_X_XXX](io_service-options.md).

### Example

```cpp
#include "yasio/yasio.hpp"

int main(){
    using namespace yasio;
    using namespace yasio::inet;
    io_hostent hosts[] = {
    {"192.168.1.66", 20336},
    {"192.168.1.88", 20337},
    };
    auto service = std::make_shared<io_service>(hosts, YASIO_ARRAYSIZE(hosts));

    // for application protocol with length field, you just needs set this option.
    // it's similar to java netty length frame based decode.
    // such as when your protocol define as following
    //    packet.header: (header.len=12bytes)
    //           code:int16_t
    //           datalen:int32_t (not contains packet.header.len)
    //           timestamp:int32_t
    //           crc16:int16_t
    //    packet.data
    service->set_option(YOPT_C_LFBFD_PARAMS,
                        0,     // channelIndex, the channel index
                        65535, // maxFrameLength, max packet size
                        2,     // lenghtFieldOffset, the offset of length field
                        4,     // lengthFieldLength, the size of length field, can be 1,2,4
                        12,    // lengthAdjustment：if the value of length feild == packet.header.len + packet.data.len, this parameter should be 0, otherwise should be sizeof(packet.header)
    );

    // for application protocol without length field, just sets length field size to -1.
    // then io_service will dispatch any packet received from server immediately,
    // such as http request, this is default behavior of channel.
    service->set_option(YOPT_C_LFBFD_PARAMS, 1, 65535, -1, 0, 0);
    return 0;
}
```

## See also

[io_event Class](./io_event-class.md)

[io_channel Class](./io_channel-class.md)

[io_service Options](./io_service-options.md)

[xxsocket Class](./xxsocket-class.md)

[obstream Class](./obstream-class.md)

[ibstream_view Class](./ibstream-class.md)

[ibstream Class](./ibstream-class.md#ibstream-class)
