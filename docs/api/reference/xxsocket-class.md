---
title: "yasio::inet::xxsocket Class"
date: "1/5/2021"
f1_keywords: ["xxsocket", "yasio/xxsocket", ]
helpviewer_keywords: []
---

# xxsocket Class

封装底层bsd socket常用API，屏蔽各操作系统平台差异。

!!! attention "注意"

    xxsocket的所有 `xxx_n` 接口均会将socket设置为非阻塞模式，且不会恢复。

## 语法

```cpp
namespace yasio { namespace inet { class xxsocket; } }
```

## 成员

|Name|Description|
|----------|-----------------|
|[xxsocket::xxsocket](#xxsocket)|构造一个 `xxsocket` 对象|

### 公共方法

|Name|Description|
|----------|-----------------|
|[xxsocket::xpconnect](#xpconnect)|建立TCP连接|
|[xxsocket::xpconnect_n](#xpconnect_n)|非阻塞方式建立TCP连接|
|[xxsocket::pconnect](#pconnect)|建立TCP连接|
|[xxsocket::pconnect_n](#pconnect_n)|非阻塞方式建立TCP连接|
|[xxsocket::pserve](#pserve)|创建tcp服务端|
|[xxsocket::swap](#swap)|交换socket描述符句柄|
|[xxsocket::open](#open)|打开socket|
|[xxsocket::reopen](#reopen)|重新打开socket|
|[xxsocket::is_open](#is_open)|检查socket是否打开|
|[xxsocket::native_handle](#native_handle)|获取socket句柄|
|[xxsocket::release_handle](#release_handle)|释放socket句柄控制权|
|[xxsocket::set_nonblocking](#set_nonblocking)|将socket设置为非阻塞模式|
|[xxsocket::test_nonblocking](#test_nonblocking)|检测socket是否为非阻塞模式|
|[xxsocket::bind](#bind)|绑定指定本地指定网卡地址|
|[xxsocket::bind_any](#bind_any)|绑定本地任意地址|
|[xxsocket::listen](#listen)|开始TCP监听|
|[xxsocket::accept](#accept)|接受一个TCP客户端连接|
|[xxsocket::accept_n](#accept_n)|非阻塞方式接受TCP连接|
|[xxsocket::connect](#connect)|建立连接|
|[xxsocket::connect_n](#connect_n)|非阻塞方式建立连接|
|[xxsocket::send](#send)|发送数据|
|[xxsocket::send_n](#send_n)|非阻塞方式发送数据|
|[xxsocket::recv](#recv)|接受数据|
|[xxsocket::recv_n](#recv_n)|非阻塞方式接受数据|
|[xxsocket::sendto](#sendto)|发送DGRAM数据到指定地址|
|[xxsocket::recvfrom](#recvfrom)|接受DGRAM数据|
|[xxsocket::handle_write_ready](#handle_write_ready)|等待socket可写|
|[xxsocket::handle_read_ready](#handle_read_ready)|等待socket可读|
|[xxsocket::local_endpoint](#local_endpoint)|获取socket本地地址|
|[xxsocket::peer_endpoint](#peer_endpoint)|获取socket远端地址|
|[xxsocket::set_keepalive](#set_keepalive)|设置tcp keepalive|
|[xxsocket::reuse_address](#reuse_address)|设置socket是否重用地址|
|[xxsocket::exclusive_address](#exclusive_address)|设置socket是否阻止地址重用|
|[xxsocket::select](#select)|监听socket事件|
|[xxsocket::shutdown](#shutdown)|停止socket收发|
|[xxsocket::close](#close)|关闭socket|
|[xxsocket::tcp_rtt](#tcp_rtt)|获取tcp rtt.|
|[xxsocket::get_last_errno](#get_last_errno)|获取最近socket错误码|
|[xxsocket::set_last_errno](#set_last_errno)|设置最近socket错误码|
|[xxsocket::strerror](#strerror)|将socket错误码转换为字符串|
|[xxsocket::gai_strerror](#gai_strerror)|将getaddrinfo返回值转换为字符串|
|[xxsocket::resolve](#resolve)|解析域名|
|[xxsocket::resolve_v4](#resolve_v4)|解析域名包含的ipv4地址|
|[xxsocket::resolve_v6](#resolve_v6)|解析域名包含的ipv6地址|
|[xxsocket::resolve_v4to6](#resolve_v4to6)|解析域名包含的ipv4地址并转换为ipv6的V4MAPPED格式|
|[xxsocket::resolve_tov6](#resolve_tov6)|解析域名包含的所有地址，ipv4地址会转换为ipv6的V4MAPPED格式|
|[xxsocket::getipsv](#getipsv)|获取本机支持的ip协议栈版本标志位|
|[xxsocket::traverse_local_address](#traverse_local_address)|枚举本机地址|


## <a name="xxsocket"></a> xxsocket::xxsocket

构造 `xxsocket` 对象。

```cpp
xxsocket::xxsocket();
xxsocket::xxsocket(socket_native_type handle);
xxsocket::xxsocket(xxsocket&& right);
xxsocket::xxsocket(int af, int type, int protocol);
```

### 参数

*handle*<br/>
通过已有socket句柄构造 `xxsocket` 对象。

*right*<br/>
move构造函数右值引用。

*af*<br/>
ip协议地址类型。

*protocol*<br/>
协议类型，对于TCP/UDP直接取 `0` 就可以。

## <a name="xpconnect"></a> xxsocket::xpconnect

和远程服务器建立TCP连接。

```cpp
int xxsocket::xpconnect(const char* hostname, u_short port, u_short local_port = 0);
```

### 参数

*hostname*<br/>
要连接服务器主机名，可以使 `IP地址` 或 `域名`。

*port*<br/>
要连接服务器的端口。

*local_port*<br/>
本地通信端口号，默认值 `0` 表示随机分配。

### 注意

会自动检测本机支持的ip协议栈。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。

## <a name="xpconnect"></a> xxsocket::xpconnect

和远程服务器建立TCP连接。

```cpp
int xxsocket::xpconnect_n(const char* hostname, u_short port, const std::chrono::microseconds& wtimeout, u_short local_port = 0);
```

### 参数

*hostname*<br/>
要连接服务器主机名，可以使 `IP地址` 或 `域名`。

*port*<br/>
要连接服务器的端口。

*local_port*<br/>
本地通信端口号，默认值 `0` 表示随机分配。

*wtimeout*<br/>
建立连接超时时间。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。

### 注意

会自动检测本机支持的ip协议栈。


## <a name="pconnect"></a> xxsocket::pconnect

和远程服务器建立TCP连接。

```cpp
int xxsocket::pconnect(const char* hostname, u_short port, u_short local_port = 0);
int xxsocket::pconnect(const endpoint& ep, u_short local_port = 0);
```

### 参数

*hostname*<br/>
要连接服务器主机名，可以使 `IP地址` 或 `域名`。

*ep*<br/>
要连接服务器的地址。

*port*<br/>
要连接服务器的端口。

*local_port*<br/>
本地通信端口号，默认值 `0` 表示随机分配。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。


### 注意

不会检测本机支持的ip协议栈。


## <a name="pconnect"></a> xxsocket::pconnect

和远程服务器建立TCP连接。

```cpp
int pconnect_n(const char* hostname, u_short port, const std::chrono::microseconds& wtimeout, u_short local_port = 0);
int pconnect_n(const char* hostname, u_short port, u_short local_port = 0);
int pconnect_n(const endpoint& ep, const std::chrono::microseconds& wtimeout, u_short local_port = 0);
int pconnect_n(const endpoint& ep, u_short local_port = 0);
```

### 参数

*hostname*<br/>
要连接服务器主机名，可以使 `IP地址` 或 `域名`。

*ep*<br/>
要连接服务器的地址。

*port*<br/>
要连接服务器的端口。

*local_port*<br/>
本地通信端口号，默认值 `0` 表示随机分配。

*wtimeout*<br/>
建立连接超时时间。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。


## <a name="pserve"></a> xxsocket::pserve

开启本地TCP服务监听。

```cpp
int pserve(const char* addr, u_short port);
int pserve(const endpoint& ep);
```

### 参数

*addr*<br/>
本机指定网卡 `IP地址`。

*ep*<br/>
本机地址。

*port*<br/>
TCP监听端口。


### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。


## <a name="swap"></a> xxsocket::swap

交换底层socket句柄。

```cpp
xxsocket& swap(xxsocket& who);
```

### 参数

*who*<br/>
交换对象。

### 返回值

`xxsocket` 左值对象的引用。


## <a name="open"></a> xxsocket::open

打开一个socket。

```cpp
bool open(int af = AF_INET, int type = SOCK_STREAM, int protocol = 0);
```

### 参数

*af*<br/>
地址类型，例如 `AF_INET` (ipv4)，`AF_INET6` (ipv6)。

*type*<br/>
socket类型， `SOCK_STREAM` (TCP), `SOCK_DGRAM` (UDP)。

*protocol*<br/>
协议，对于TCP/UDP，取 `0` 即可。


### 返回值

`true`: 成功， `false`失败，通过 `xxsocket::get_last_errno` 获取错误码。

## <a name="open"></a> xxsocket::open

打开一个socket。

```cpp
bool reopen(int af = AF_INET, int type = SOCK_STREAM, int protocol = 0);
```

### 参数

*af*<br/>
地址类型，例如 `AF_INET` (ipv4)，`AF_INET6` (ipv6)。

*type*<br/>
socket类型， `SOCK_STREAM` (TCP), `SOCK_DGRAM` (UDP)。

*protocol*<br/>
协议，对于TCP/UDP，取 `0` 即可。


### 返回值

`true`: 成功， `false`失败，通过 `xxsocket::get_last_errno` 获取错误码。

### 注意

如果socket已打开，此次函数会先关闭，再重新打开。


## <a name="is_open"></a> xxsocket::is_open

判断socket是否已打开。

```cpp
bool is_open(void) const;
```

### 返回值

`true`: 已打开， `false`: 未打开。


## <a name="native_handle"></a> xxsocket::native_handle

获取socket文件描述符。

```cpp
socket_native_type native_handle(void) const;
```

### 返回值

socket文件描述符，`yasio::inet::invalid_socket` 表示无效socket。

## <a name="native_handle"></a> xxsocket::release_handle

释放底层socket描述符控制权。

```cpp
socket_native_type release_handle(void) const;
```

### 返回值

释放前的socket文件描述符

## <a name="set_nonblocking"></a> xxsocket::set_nonblocking

设置socket的非阻塞模式。

```cpp
int set_nonblocking(bool nonblocking) const;
```

### 参数

*nonblocking*<br/>
`true`: 非阻塞模式，`false`: 阻塞模式。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。

## <a name="test_nonblocking"></a> xxsocket::test_nonblocking

检测socket是否为非阻塞模式。

```cpp
int test_nonblocking() const;
```

### 返回值

`1`: 非阻塞模式， `0`: 阻塞模式。

### 注意

对于winsock2，未连接的 `SOCK_STREAM` 类型socket会返回 `-1`。

## <a name="bind"></a> xxsocket::bind

绑定socket本机地址。

```cpp
int bind(const char* addr, unsigned short port) const;
int bind(const endpoint& ep) const;
```

### 参数

*addr*<br/>
本机指定网卡ip地址。

*port*<br/>
要绑定的端口。

*ep*<br/>
要绑定的本机地址。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。

## <a name="bind_any"></a> xxsocket::bind_any

绑定socket本机任意地址。

```cpp
int bind_any(bool ipv6) const;
```

### 参数

*ipv6*<br/>
是否绑定本机任意IPv6地址

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。


## <a name="listen"></a> xxsocket::listen

开始监听来自tcp客户端的握手请求。

```cpp
int listen(int backlog = SOMAXCONN) const;
```

### 参数

*backlog*<br/>
最大监听数。

### 返回值

`0`: 成功， `< 0`失败，通过 `xxsocket::get_last_errno` 获取错误码。

## <a name="accept"></a> xxsocket::accept

接受一个客户端连接。

```cpp
xxsocket accept(socklen_t addrlen = sizeof(sockaddr));
```

### 参数

*addrlen*<br/>
地址大小。

### 返回值

和客户端通信的 `xxsocket` 对象。


## 请参阅

[io_service Class](./io_service-class.md)
