---
title: "yasio::inet::xxsocket Class"
date: "1/5/2021"
f1_keywords: ["xxsocket", "yasio/xxsocket", ]
helpviewer_keywords: []
---

# xxsocket Class

Provides the functionality of low-level socket based on POSIX socket APIs, support std::move

## Syntax

```cpp
namespace yasio { namespace inet { class xxsocket; } }
```

## Members

|Name|Description|
|----------|-----------------|
|[xxsocket::xxsocket](#xxsocket)|Constructs a `xxsocket` object.|

### Public Methods

|Name|Description|
|----------|-----------------|
|[xxsocket::xpconnect](#xpconnect)|Cnnect remote via tcp.|
|[xxsocket::xpconnect_n](#xpconnect_n)|Connect remote via tcp non-blocking.|
|[xxsocket::pconnect](#pconnect)|Connect remote via tcp.|
|[xxsocket::pconnect_n](#pconnect_n)|Connect remote via tcp non-blocking.|
|[xxsocket::pserv](#pserv)|Create socket as tcp server.|
|[xxsocket::swap](#swap)|Swap socket handle.|
|[xxsocket::open](#open)|Open a socket.|
|[xxsocket::reopen](#reopen)|Reopen a socket.|
|[xxsocket::is_open](#is_open)|Check whether socket opened.|
|[xxsocket::native_handle](#native_handle)|Gets socket handle.|
|[xxsocket::detach](#detach)|Detach socket handle.|
|[xxsocket::set_nonblocking](#set_nonblocking)|Sets socket non-blocking mode.|
|[xxsocket::test_nonblocking](#test_nonblocking)|Test whether socket is non-blocking mode.|
|[xxsocket::bind](#bind)|Bind socket with specific address.|
|[xxsocket::bind_any](#bind_any)|Bind socket with any address.|
|[xxsocket::listen](#listen)|Listen a tcp socket.|
|[xxsocket::accept](#accept)|Accept a tcp socket.|
|[xxsocket::accept_n](#accept_n)|Accept a tcp socket non-blocking.|
|[xxsocket::connect](#connect)|Connect a socket.|
|[xxsocket::connect_n](#connect_n)|Connect a socket non-blocking.|
|[xxsocket::send](#send)|Send data on the socket.|
|[xxsocket::send_n](#send_n)|Send data on the socket non-blocking.|
|[xxsocket::recv](#recv)|Receive data from the socket.|
|[xxsocket:recv_n](#recv_n)|Receive data from the socket non-blocking.|
|[xxsocket:sendto](#sendto)|Send data to a DGRAM socket.|
|[xxsocket:recvfrom](#recvfrom)|Send data to a DGRAM socket non-blocking.|
|[xxsocket::handle_write_ready](#handle_write_ready)|Wait socket ready to write.|
|[xxsocket::handle_read_ready](#handle_read_ready)|Wait socket ready to read.|
|[xxsocket::local_endpoint](#local_endpoint)|Gets local endpoint of socket.|
|[xxsocket::peer_endpoint](#peer_endpoint)|Gets peer endpoint of socket.|
|[xxsocket::set_keepalive](#set_keepalive)|Sets tcp socket keepalive.|
|[xxsocket::reuse_address](#reuse_address)|Sets socket reuse address.|
|[xxsocket::exclusive_address](#exclusive_address)|Sets socket exclusive address.|
|[xxsocket::select](#select)|Select event ready for socket.|
|[xxsocket::alive](#alive)|Check whether socket status is ok.|
|[xxsocket::shutdown](#shutdown)|Shutdown socket.|
|[xxsocket:close](#close)|Close socket.|
|[xxsocket:tcp_rtt](#tcp_rtt)|Gets tcp socket rtt.|
|[xxsocket:get_last_errno](#get_last_errno)|Gets last socket error.|
|[xxsocket:set_last_errno](#set_last_errno)|Sets last socket error.|
|[xxsocket::strerror](#strerror)|Translate socket error code to string.|
|[xxsocket::gai_strerror](#gai_strerror)|Translate getaddrinfo error code to string.|
|[xxsocket::resolve](#resolve)|Resolve domain.|
|[xxsocket::resolve_v4](#resolve_v4)|Resolve domain ipv4 address.|
|[xxsocket::resolve_v6](#resolve_v6)|Resolve domain ipv6 address.|
|[xxsocket::resolve_v4to6](#resolve_v4to6)|Resolve ipv4 address and convert to ipv6 V4MAPPED format.|
|[xxsocket::resolve_tov6](#resolve_tov6)|Resolve all address, convert ipv4 address to ipv6 V4MAPPED format.|
|[xxsocket::getipsv](#getipsv)|Get local supported ip stack flags.|
|[xxsocket::traverse_local_address](#traverse_local_address)|Traverse local address.|


## See also

[io_service Class](./io_service-class.md)
