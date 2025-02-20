# go-nat-discovery

NAT type discovery tool using STUN written purely in Go, include client and server.

Rewrite the [go-nats](https://github.com/enobufs/go-nats) which is powered by [pion](https://pion.ly).
Add features:
- fix bugs in original go-nats' discover rules
- add stun discovery server 
- add slave and master server, for servers don't have two public ip

本repo改写了 [go-nats](https://github.com/enobufs/go-nats) ,加入了以下功能

- 修正原repo里的discover rules的bugs
- 增加stun discovery server
- 如果server没有2个公网ip，本repo实现了master和slave模式，可以在不同机器上运行他们实现同样的效果

## Usage

### client 

```
# go run client.go -H stun.sipgate.net -P 3478 -m localIp:localPort -f anotherLocalIp:anotherLocalPort
```

should has output:
```
NAT Type: Blocked
External IP: None
External Port: None
```

### server

#### server has two public ip

```
# go run server.go -p publicIp-1:port-1 -s publicIP-2:port-2
```

#### server has one public ip

If you don't have two public ip on one server, then You must have two server, each one has one public ip, and primary server can communicate to secondary server via `primary2SecondaryHost:port`.

- server A, run as primary
```
# go run server.go -r pri -p publicIpOnPrimary:portA -s publicIpOnServerB:portB -p2s primary2SecondaryHost:port
```

- server B, run as secondary
```
# go run server.go -r sec -p publicIpOnPrimary:portA -s publicIpOnServerB:portB -p2s primary2SecondaryHost:port
```

