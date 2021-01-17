# yasio 粘包处理

yasio的粘包处理已经不仅仅针对TCP，对于UDP传输协议，如果发送端有组包发送机制，也是以相同的方式处理。整体来讲有两种方式: <br/>

- 通过io_service选项 [YOPT_C_LFBFD_PARAMS](#lfbfd_params) 设置信道参数。
- 通过io_service选项 `YOPT_C_LFBFD_FN` 设置自定义包长度解码函数 [decode_len_fn_t](#decode_len_fn_t) 。

## <a name="lfbfd_params"></a> YOPT_C_LFBFD_PARAMS

通过设置信道拆包参数。

### 参数

*max_frame_length*<br/>
最大包长度，超过将视为异常包。

*length_field_offset*<br/>
包长度字段相对于消息包数据首字节偏移。

*length_field_length*<br/>
包长度字段长度，支持1~4字节。

*length_adjustment*<br/>
包长度矫正数值。通常应用层二进制协议都会设计消息头和消息体，当长度字段值包含消息头时，则此值为 `0`，否则为 `消息头长度`。

### 注意

消息包长度字段必须使用网络字节序编码。请查看内置解码长度实现:

```cpp
int io_channel::__builtin_decode_len(void* d, int n)
{
  int loffset = uparams_.length_field_offset;
  int lsize   = uparams_.length_field_length;
  if (loffset >= 0)
  {
    int len = 0;
    if (n >= (loffset + lsize))
    {
      ::memcpy(&len, (uint8_t*)d + loffset, lsize);
      len = yasio::network_convert_traits::fromint(len, lsize);
      len += uparams_.length_adjustment;
      if (len > uparams_.max_frame_length)
        len = -1;
    }
    return len;
  }
  return n;
}
```

## decode_len_fn_t

信道解码函数原型。

```cpp
typedef std::function<int(void* d, int n)> decode_len_fn_t;
```

### 参数

*d*<br/>
待解包数据首字节地址。

*n*<br/>
待解包数据长度。

### 返回值

返回消息包数据实际长度。

- `> 0`: 解包长度成功。
- `== 0`: 接收数据不足以获取消息包实际长度，yasio底层会继续接收数据。
- `< 0`: 解码数据包长度异常，会触发当前传输会话断开。
