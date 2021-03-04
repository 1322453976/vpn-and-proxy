咨询：QQ361499671

#自定义VPN协议
- 支持UDP协议和FAKE TCP协议，两种协议模式
- 运行环境：centos7

1、设置校验数据包反向路径级别
- sysctl net.ipv4.conf.all.rp_filter=2
- sysctl net.ipv4.conf.default.rp_filter=2

2、服务的运行配置
- ./cm-vpn --config server.conf &
- 查询接入客户端信息命令：./cm-vpn 9000 list
- 强制客户端用户下线命令：./cm-vpn 9000 kill 10.10.10.130
- 说明:此处9000端口是clinet.conf配置文件中的manage端口号。

3、客户端运行配置
- ./cm-vpn --config client.conf &
- 拨号命令：./cm-vpn 9000 dial
- 说明:此处9000端口是clinet.conf配置文件中的manage端口号。
- 停止vpn命令：./cm-vpn 9000 stop

4、依赖库
- libjson-c.so

#

#自定义代理模式，支持UDP协议和TCP协议代理
- 运行环境：centos7


1、启动代理客户端
- ./proxy-client --config client.conf &

2、启动代理服务端
- ./proxy-server --config server.conf &

- 使用udp协议，链接management端口。
- 在centos环境上可以直接使用nc -u localhost management-port

3、代理客户端配置：
- 配置转发规则：
{"checksum":"400af464c76d713c07ad25d55ad283aa", "checkValue":"12345678", "data":{"manage":2,"ctrl":2,"flowRule":1}}

- 配置代理服务端信息：
{"checksum":"400af464c76d713c07ad25d55ad283aa", "checkValue":"12345678", "data":{"manage":0,"ctrl":0,"username":"test","password":"2020","user-seq":0,"proxy-server":"95.66.66.66","proxy-tcp-port":19090,"proxy-udp-port":19090,"dev":["192.168.1.198"]}}

- 说明：此处dev指需要代理的设备IP地址，最多支持配置两台设备。

4、代理服务器查询用户信息：
- {"ctrl":3,"username":"test"}

5、依赖库
- libjson-c.so
- libmnl.so.0
- libnetfilter_conntrack.so.3
- libnfnetlink.so.0