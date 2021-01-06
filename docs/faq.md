## FAQ

??? question "在iOS设备上连接服务器端口号为10161时失败，错误信息：ec=65, detail:No route to host"

    - 根本原因:
        - 当APP初次启动时，在ios14.1+设备上，会提示请求 `Local Network Perssion` 权限，用户点拒绝。
        - 端口号 10161 时系统保留端口，用于 `SNMP via TLS` 协议。 
    - 解决方案: 使用其他端口号作为APP服务端口。

??? question "Can't load xlua bundle on macOS?" 

    The file `xlua.bundle` needs change attr by command `sudo xattr -r -d com.apple.quarantine xlua.bundle`  
