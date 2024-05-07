https://bronya.io/chat-to-api-solving-429/
使用TunnelBroker提供的免费/48的ipv6解决chat-to-api的429问题
配置隧道
首先你要有一个拥有公网IPv4，且防火墙允许ICMP、TCP、6in4协议通过的VPS，大部分的系统应该都是可以的，下文以ubuntu 22.04为例。

注册一个TunnelBroker的账号 https://tunnelbroker.net/

注册成功之后创建一个隧道。（注意新号注册当天无法申请/48，可能需要等一天）

成功创建隧道之后，找到Example Configurations，找到你的系统的配置样例，配置到你的系统上，因为我是用的是ubuntu 22.04，这个系统当前使用的是netplan 管理网络，因此我选择的是netplan

![image](https://github.com/CattleZoe/IPv6-proxy/assets/5591163/ea64a243-6c09-4a36-9f17-2051219bd674)



配置好了之后，你的服务器因该就有了一个ipv6地址了。可以通过这个命令来进行测试

复制成功
curl ipv6.ip.sb
如果这里不通，说明你配置的有问题，请回去检查配置。注意一下local可能是你的公网ip，也可能是内网ip，和服务商配置有关，一般都是公网ip，甲骨文是内网ip。

这样你就至少有了一个/64的ipv6地址了，等一天之后你可以点一下申请/48的地址。

启动代理服务
有了地址之后，我们需要一个程序把这些地址变成一个标准的代理服务，方便chat to api使用。可以使用我刚写的这个，可以把ipv6地址段变成一个随机的代理池，使用方法我都放在readme中了。其中的--cidr就是在上述文章中申请到的/64 或者/48的地址段。

https://github.com/zbronya/v6-proxy

其他说明
不限于chatgpt，只要是有ipv6解析的有ip限制的服务都是可以用的
代理程序是通用的，如果不喜欢TunnelBroker，或者你有其他的/32 /29的地址，也是可以使用的
代理程序我只在ubuntu 22.04的amd和arm系统上测试过，其他的系统如果有问题可以提issue或者pr
帮我程序点个star吧
如果你还没有chat-to-api的项目，也可以试试我的 https://github.com/zbronya/free-chat-to-api


# v6-proxy
A random IPv6 proxy service based on Go

## Installation

```bash
curl -sSL https://github.com/zbronya/v6-proxy/raw/master/install.sh | sudo bash
```

## Usage
```bash
sudo v6-proxy --cidr=2001:db8::/32
```
Please replace the --cidr parameter with your IPv6 address range.


test
```bash
while true; do curl -x http://127.0.0.1:33300 -s https://api.ip.sb/ip -A Mozilla; done
```


## Parameters
- `--cidr` ipv6 address segment, required
- `--port` proxy server port, default 33300
- `--username` proxy server username, default empty
- `--password` proxy server password, default empty
- `--bind` proxy server bind address, default 127.0.0.1
- `--auto-route` auto add route to the system, default true **need root permission**
- `--auto-forwarding` auto add forwarding to the system, default true **need root permission**

## what's next
- [使用TunnelBroker提供的免费/48的ipv6解决chat-to-api的429问题](https://bronya.io/chat-to-api-solving-429/)





