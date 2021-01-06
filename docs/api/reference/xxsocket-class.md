---
title: "yasio::inet::xxsocket Class"
date: "1/5/2021"
f1_keywords: ["xxsocket", "yasio/xxsocket", ]
helpviewer_keywords: []
---

# xxsocket Class

封装底层bsd socket常用API，屏蔽各操作系统平台差异。

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
|[xxsocket::pserv](#pserv)|创建tcp服务端|
|[xxsocket::swap](#swap)|交换socket描述符句柄|
|[xxsocket::open](#open)|打开socket|
|[xxsocket::reopen](#reopen)|重新打开socket|
|[xxsocket::is_open](#is_open)|检查socket是否打开|
|[xxsocket::native_handle](#native_handle)|获取socket句柄|
|[xxsocket::detach](#detach)|解除socket句柄管理|
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


## 请参阅

[io_service Class](./io_service-class.md)
