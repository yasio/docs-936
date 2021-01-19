# yasio Interop
为了支持Unity C#， yasio提供了C语言接口导出，详见: https://github.com/yasio/yasio/blob/master/yasio/bindings/yasio_ni.cpp

## dotnet API

```csharp
class NSM {
const string LIBNAME = "xlua";

public delegate void YNIEventDelegate(uint emask, int cidx, IntPtr sid, IntPtr bytes, int len);
public delegate int YNIResolvDelegate(string host, IntPtr sbuf);
public delegate void YNIPrintDelegate(int level, string msg);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_init_globals(YNIPrintDelegate fnPrint);

/// <summary>
/// Start a low level socket io service
/// </summary>
/// <param name="strParam">
/// format: "ip:port;ip:port;ip:port"
/// </param>
/// <param name="d"></param>
/// <returns></returns>
[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_start(int channel_count, YNIEventDelegate d);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_open(int cindex, int cmask);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_close(int cindex);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_close_handle(IntPtr sid);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_write(IntPtr sid, byte[] bytes, int len);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern uint yasio_tcp_rtt(IntPtr sid);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_dispatch(int count);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_set_option(int opt, string strParam);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_set_resolv_fn(YNIResolvDelegate fnResolv);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_set_print_fn(YNIPrintDelegate fnPrint);

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_stop();

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern long yasio_highp_time();

[DllImport(LIBNAME, CallingConvention = CallingConvention.Cdecl)]
public static extern void yasio_memcpy(IntPtr destination, IntPtr source, uint length);

/// <summary>
/// The yasio constants
/// </summary>
public enum YEnums
{
    #region Channel mask enums, copy from yasio.hpp
    YCM_CLIENT = 1,
    YCM_SERVER = 1 << 1,
    YCM_TCP = 1 << 2,
    YCM_UDP = 1 << 3,
    YCM_KCP = 1 << 4,
    YCM_SSL = 1 << 5,
    YCK_TCP_CLIENT = YCM_TCP | YCM_CLIENT,
    YCK_TCP_SERVER = YCM_TCP | YCM_SERVER,
    YCK_UDP_CLIENT = YCM_UDP | YCM_CLIENT,
    YCK_UDP_SERVER = YCM_UDP | YCM_SERVER,
    YCK_KCP_CLIENT = YCM_KCP | YCM_CLIENT | YCM_UDP,
    YCK_KCP_SERVER = YCM_KCP | YCM_SERVER | YCM_UDP,
    YCK_SSL_CLIENT = YCM_SSL | YCM_CLIENT | YCM_TCP,
    #endregion

    #region Event kind enums, copy from yasio.hpp
    YEK_CONNECT_RESPONSE = 1,
    YEK_CONNECTION_LOST,
    YEK_PACKET,
    #endregion

    #region Channel flags
    /* Whether setsockopt SO_REUSEADDR and SO_REUSEPORT */
    YCF_REUSEADDR = 1 << 9,

    /* For winsock security issue, see:
       https://docs.microsoft.com/en-us/windows/win32/winsock/using-so-reuseaddr-and-so-exclusiveaddruse
    */
    YCF_EXCLUSIVEADDRUSE = 1 << 10,
    #endregion

    #region All supported options by native, copy from yasio.hpp
    // Set whether deferred dispatch event.
    // params: deferred_event:int(1)
    YOPT_S_DEFERRED_EVENT = 1,
  
    // Set custom resolve function, native C++ ONLY.
    // params: func:resolv_fn_t*
    // remarks: you must ensure thread safe of it.
    YOPT_S_RESOLV_FN,
  
    // Set custom print function, native C++ ONLY.
    // parmas: func:print_fn_t
    // remarks: you must ensure thread safe of it.
    YOPT_S_PRINT_FN,
  
    // Set custom print function, native C++ ONLY.
    // parmas: func:print_fn2_t
    // remarks: you must ensure thread safe of it.
    YOPT_S_PRINT_FN2,
  
    // Set event callback
    // params: func:event_cb_t*
    // remarks: this callback will be invoke at io_service::dispatch caller thread
    YOPT_S_EVENT_CB,
  
    // Sets callback before defer dispatch event.
    // params: func:defer_event_cb_t*
    // remarks: this callback invoke at io_service thread
    YOPT_S_DEFER_EVENT_CB,
  
    // Set tcp keepalive in seconds, probes is tries.
    // params: idle:int(7200), interal:int(75), probes:int(10)
    YOPT_S_TCP_KEEPALIVE,
  
    // Don't start a new thread to run event loop
    // params: value:int(0)
    YOPT_S_NO_NEW_THREAD,
  
    // Sets ssl verification cert, if empty, don't verify
    // params: path:const char*
    YOPT_S_SSL_CACERT,
  
    // Set connect timeout in seconds
    // params: connect_timeout:int(10)
    YOPT_S_CONNECT_TIMEOUT,
  
    // Set dns cache timeout in seconds
    // params: dns_cache_timeout : int(600),
    YOPT_S_DNS_CACHE_TIMEOUT,
  
    // Set dns queries timeout in milliseconds, default is: 5000
    // params: dns_queries_timeout : int(5000)
    // remarks:
    //         a. this option must be set before 'io_service::start'
    //         b. only works when have c-ares
    //         c. since v3.33.0 it's milliseconds, previous is seconds.
    //         d. the timeout algorithm of c-ares is complicated, usually, by default, dns queries
    //         will failed with timeout after more than 75 seconds.
    //         e. for more detail, please see:
    //         https://c-ares.haxx.se/ares_init_options.html
    YOPT_S_DNS_QUERIES_TIMEOUT,
  
    // [DEPRECATED], same with YOPT_S_DNS_QUERIES_TIMEOUT
    YOPT_S_DNS_QUERIES_TIMEOUTMS,
  
    // Set dns queries tries when timeout reached, default is: 5
    // params: dns_queries_tries : int(5)
    // remarks:
    //        a. this option must be set before 'io_service::start'
    //        b. relative option: YOPT_S_DNS_QUERIES_TIMEOUT
    YOPT_S_DNS_QUERIES_TRIES,
  
    // Set dns server dirty
    // params: reserved : int(1)
    // remarks:
    //        a. this option only works with c-ares enabled
    //        b. you should set this option after your mobile network changed
    YOPT_S_DNS_DIRTY,
  
    // Sets channel length field based frame decode function, native C++ ONLY
    // params: index:int, func:decode_len_fn_t*
    YOPT_C_LFBFD_FN = 101,
  
    // Sets channel length field based frame decode params
    // params:
    //     index:int,
    //     max_frame_length:int(10MBytes),
    //     length_field_offset:int(-1),
    //     length_field_length:int(4),
    //     length_adjustment:int(0),
    YOPT_C_UNPACK_PARAMS,
    YOPT_C_LFBFD_PARAMS = YOPT_C_UNPACK_PARAMS,
  
    // Sets channel length field based frame decode initial bytes to strip
    // params:
    //     index:int,
    //     initial_bytes_to_strip:int(0)
    YOPT_C_UNPACK_STRIP,
    YOPT_C_LFBFD_IBTS = YOPT_C_UNPACK_STRIP,
  
    // Sets channel remote host
    // params: index:int, ip:const char*
    YOPT_C_REMOTE_HOST,
  
    // Sets channel remote port
    // params: index:int, port:int
    YOPT_C_REMOTE_PORT,
  
    // Sets channel remote endpoint
    // params: index:int, ip:const char*, port:int
    YOPT_C_REMOTE_ENDPOINT,
  
    // Sets local host for client channel only
    // params: index:int, ip:const char*
    YOPT_C_LOCAL_HOST,
  
    // Sets local port for client channel only
    // params: index:int, port:int
    YOPT_C_LOCAL_PORT,
  
    // Sets local endpoint for client channel only
    // params: index:int, ip:const char*, port:int
    YOPT_C_LOCAL_ENDPOINT,
  
    // Mods channl flags
    // params: index:int, flagsToAdd:int, flagsToRemove:int
    YOPT_C_MOD_FLAGS,
  
    // Enable channel multicast mode
    // params: index:int, multi_addr:const char*, loopback:int
    YOPT_C_ENABLE_MCAST,
  
    // Disable channel multicast mode
    // params: index:int
    YOPT_C_DISABLE_MCAST,
  
    // The kcp conv id, must equal in two endpoint from the same connection
    // params: index:int, conv:int
    YOPT_C_KCP_CONV,
  
    // Change 4-tuple association for io_transport_udp
    // params: transport:transport_handle_t
    // remarks: only works for udp client transport
    YOPT_T_CONNECT,
  
    // Dissolve 4-tuple association for io_transport_udp
    // params: transport:transport_handle_t
    // remarks: only works for udp client transport
    YOPT_T_DISCONNECT,
    #endregion
};
}
```
