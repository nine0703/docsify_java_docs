[centos7](https://so.csdn.net/so/search?q=centos7&spm=1001.2101.3001.7020) 自带防火墙是firewalld。在某下情况下可能导致docker 的某些网络问题。
[docker](https://so.csdn.net/so/search?q=docker&spm=1001.2101.3001.7020) 有4种网络模式：
1.bridge模式（默认）：[网桥](https://so.csdn.net/so/search?q=%E7%BD%91%E6%A1%A5&spm=1001.2101.3001.7020)模式，通过虚拟网桥使容器通信。容器有自己的独立ip和端口。
2.host模式：主机模式，与主机共用一个网络，容器ip是主机的ip，端口占用主机的端口范围。
3.[container](https://so.csdn.net/so/search?q=container&spm=1001.2101.3001.7020)模式：与指定容器共享一个网络，类似host模式，但是是两个容器间共用一个ip。
4.none模式：无网络模式，容器有自己的内部网络，但是没有分配ip，路由等信息，需要自己分配。

默认是bridge 网桥模式，docker会在宿住机配置一个虚拟网卡，并将容器连接到该网卡，而docker网络与宿住机外部网络的通信，是借助nat来实现的。所以当Iptable出现问题时，就会导致docker容器网络异常

### [](#)[](#)查看firewalld 日志

```
`tail -10 /var/log/firewalld` 

- 1


```

在日志中可以看到一些警告信息，是docker添加的nat规则，但是因为chain不存在，所以添加失败。一般情况下是没有什么问题，但是某些情况，比如容器内需要访问宿住机的服务，则可能出现问题。

```
`2019-10-12 17:21:43 WARNING: COMMAND_FAILED: '/usr/sbin/iptables -w10 -t filter -X DOCKER-ISOLATION' failed: iptables: No chain/target/match by that name.

2019-10-12 17:21:43 WARNING: COMMAND_FAILED: '/usr/sbin/iptables -w10 -D FORWARD -i docker0 -o docker0 -j DROP' failed: iptables: Bad rule (does a matching rule exist in that chain?).

2019-10-12 17:21:43 WARNING: COMMAND_FAILED: '/usr/sbin/iptables -w10 -D FORWARD -i br-7407bd71b6f0 -o br-7407bd71b6f0 -j DROP' failed: iptables: Bad rule (does a matching rule exist in that chain?).` 

```

### [](#)[](#)解决方案：

#### [](#)1.修改docker网络模式为host

在运行容器时添加参数 --net=host，但是不能从根本上解决问题。

```
`docker run -d --net=host --name redis redis` 

- 1
```

#### [](#)2.将firewalld换成[iptables](https://so.csdn.net/so/search?q=iptables&spm=1001.2101.3001.7020)（推荐）

```
`#查看firewalld是否启用
systemctl status firewalld
#停止firewalld
systemctl stop firewalld
#禁用firewalld（否则重启系统后会再次启动）
systemctl disable firewalld
#查看是否安装iptables
yum list installed | grep iptables-services
#如果没安装则安装下
yum install iptables-services -y
#重启iptables
systemctl restart iptables
#设置开机自启
systemctl enable iptables
#重启docker
systemctl restart docker`
```

#### [](#)3.关闭防火墙

不推荐，特别是在生产环境

```
`#查看firewalld是否启用
systemctl status firewalld
#停止firewalld
systemctl stop firewalld
#禁用firewalld（否则重启系统后会再次启动）
systemctl disable firewalld`
```