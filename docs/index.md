# yasio Documentation

[![Release](https://img.shields.io/badge/release-v3.36.0-blue.svg)](https://github.com/yasio/yasio/releases)
[![GitHub stars](https://img.shields.io/github/stars/yasio/yasio.svg?label=Stars)](https://github.com/yasio/yasio)
[![Docs Status](https://readthedocs.org/projects/yasio-docs-437/badge/?version=latest)](https://readthedocs.org/projects/yasio-docs-437)

*yasio is a multi-platform support c++11 library with focus on asio (asynchronous socket I/O) for any client application, support windows & linux & apple & android & win10-universal.*

- Languages:
    - [English](https://docs.yasio.org/en/latest/)
    - [简体中文](https://docs.yasio.org/zh_CN/latest/)
- Cross-platform:
    - Compiler: 
        - Visual Studio 2013+
        - GCC4.7+
        - xcode9+
        - Other C++11,14,17 Compilers

    - Architecture: x86, x64, ARM and etc.
    - OS: Windows, macOS, Linux, FreeBSD, iOS, Android And etc.

## Quick Start
This demo simply send http request to ``tool.chinaz.com`` and print resposne data.

=== "C++"

    ```cpp
    #include "yasio/yasio.hpp"
    #include "yasio/obstream.hpp"
    using namespace yasio;
    using namespace yasio::inet;
    int main()
    {
        io_service service({"tool.chinaz.com", 80});
        service.set_option(YOPT_S_DEFERRED_EVENT, 0); // dispatch network event on network thread
        service.start([&](event_ptr&& ev) {
            switch (ev->kind())
            {
            case YEK_PACKET: {
                auto packet = std::move(ev->packet());
                fwrite(packet.data(), packet.size(), 1, stdout);
                fflush(stdout);
                break;
            }
            case YEK_CONNECT_RESPONSE:
                if (ev->status() == 0)
                {
                auto transport = ev->transport();
                if (ev->cindex() == 0)
                {
                    obstream obs;
                    obs.write_bytes("GET /index.htm HTTP/1.1\r\n");

                    obs.write_bytes("Host: tool.chinaz.com\r\n");

                    obs.write_bytes("User-Agent: Mozilla/5.0 (Windows NT 10.0; "
                                    "WOW64) AppleWebKit/537.36 (KHTML, like Gecko) "
                                    "Chrome/87.0.4820.88 Safari/537.36\r\n");
                    obs.write_bytes("Accept: */*;q=0.8\r\n");
                    obs.write_bytes("Connection: Close\r\n\r\n");

                    service.write(transport, std::move(obs.buffer()));
                }
                }
                break;
            case YEK_CONNECTION_LOST:
                printf("The connection is lost.\n");
                break;
            }
        });
        // open channel 0 as tcp client
        service.open(0, YCK_TCP_CLIENT);
        getchar();
    }
    ```

=== "Lua"

    ```lua
    local ip138 = "tool.chinaz.com"
    local service = yasio.io_service.new({host=ip138, port=80})
    local respdata = ""
    service:start(function(ev)
            local k = ev.kind()
            if (k == yasio.YEK_PACKET) then
                respdata = respdata .. ev:packet():to_string()
            elseif k == yasio.YEK_CONNECT_RESPONSE then
                if ev:status() == 0 then -- connect succeed
                    local transport = ev:transport()
                    local obs = yasio.obstream.new()
                    obs:write_bytes("GET / HTTP/1.1\r\n")

                    obs:write_bytes("Host: " .. ip138 .. "\r\n")

                    obs:write_bytes("User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117 Safari/537.36\r\n")
                    obs:write_bytes("Accept: */*;q=0.8\r\n")
                    obs:write_bytes("Connection: Close\r\n\r\n")

                    service:write(transport, obs)
                end
            elseif k == yasio.YEK_CONNECTION_LOST then
                print("request finish, respdata: " ..  respdata)
            end
        end)
    -- Open channel 0 as tcp client and start non-blocking tcp 3 times handshake
    service:open(0, yasio.YCK_TCP_CLIENT)

    -- Call this function at thread which focus on the network event.
    function gDispatchNetworkEvent(...)
        service:dispatch(128) -- dispatch max events is 128 per frame
    end

    _G.yservice = service -- Store service to global table as a singleton instance
    ```

## The [tests](https://github.com/yasio/yasio/tree/master/tests) & [examples](https://github.com/yasio/yasio/tree/master/tests):

* tests:
    * [echo_server](https://github.com/yasio/yasio/tree/master/tests/echo_server): TCP/UDP/KCP echo server
    * [echo_client](https://github.com/yasio/yasio/tree/master/tests/echo_client): TCP/UDP/KCP echo client
    * [ssltest](https://github.com/yasio/yasio/tree/master/tests/ssl): SSL client test, Get github.com home page
    * [tcptest](https://github.com/yasio/yasio/tree/master/tests/tcp): TCP test
    * [speedtest](https://github.com/yasio/yasio/tree/master/tests/speed): TCP,UDP,KCP local transfer
    * [mcast](https://github.com/yasio/yasio/tree/master/tests/mcast): multi-cast test program

* examples:
    * [ftp_server](https://github.com/yasio/ftp_server): A simple ftp server only support file download which is based on yasio，[click](ftp://ftp.yasio.org/) to visit.
    * [lua](https://github.com/yasio/yasio/tree/master/examples/lua): lua test contains http request，TCP unpack test code.
    * [xlua](https://github.com/yasio/xLua): Unity3D xlua Integration Demo.
    * [DemoUE4](https://github.com/yasio/DemoUE4): Unreal Engine 4 Integration Demo.

## Build tests & examples
* Ensure install compiler which support C++11, such as ``msvc``, ``gcc``, ``clang``
* Ensure ``git``, ``cmake`` installed
* Execude follow commands:

```sh
  git clone https://github.com/yasio/yasio
  cd yasio
  git submodule update --init --recursive 
  cd build
  # for xcode should be: cmake .. -GXcode
  cmake ..
  cmake --build . --config Debug
```

## API documentation
* [API References](api/index.md)
* [FAQ](faq.md)
