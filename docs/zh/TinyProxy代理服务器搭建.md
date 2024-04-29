#### 安装Tinyproxy

tinyproxy版本1.8.3不支持账号密码验证，而版本1.10以上支持

1、如果不需要鉴权，可以直接通过`yum install tinyproxy`安装默认的1.8版本

2、如果需要鉴权，则要使用1.10以上版本

##### 1.直接安装TinyProxy

```
yum install -y epel-release
yum update -y
yum install -y tinyproxy
```

##### 2.从源码编译安装

```
wget -P /opt/software https://github.com/tinyproxy/tinyproxy/releases/download/1.11.0-rc1/tinyproxy-1.11.0-rc1.tar.gz

```



```
nano /etc/tinyproxy/tinyproxy.conf
```

修改两处配置：端口号和允许连接该代理的IP白名单：



![img](images/ac6eddc451da81cb753bed17a363911d0824311f.png@f_auto)



上图中，默认用8888端口作为代理端口，可以修改为你用作代理端口的端口号；



![img](images/bba1cd11728b47106158765e33cb82f6fc03233a.png@f_auto)



允许访问该代理端口的IP，默认为只有本机可以使用该代理，把这行注释掉可以使所有IP都可以使用该代理。

（2）配置完后，重启TinyProxy

```
systemctl enable tinyproxy.service
systemctl restart tinyproxy.service
```

防火墙开放你的配置的代理端口：

```
iptables -I INPUT -p tcp --dport 8888 -j ACCEPT
```

在另一台主机上运行以下命令检查是否用了代理：

```
curl -x 代理服务器IP地址:代理端口 httpbin.org/get
```

![img](images/faf2b2119313b07e2e42340afcd2d02896dd8cc2.png@f_auto)



上图中访问httpbin.org/get得到本地IP为119.23.xx.x，通过代理端口访问该地址，得到的IP地址为114.104.xx.x，这说明代理搭建成功。



#### 方法2

##### 安装Tinyproxy

tinyproxy版本1.8.3不支持账号密码验证，而版本1.10以上支持

1、如果不需要鉴权，可以直接通过`yum install tinyproxy`安装默认的1.8版本

2、如果需要鉴权，则要使用1.10以上版本

这里选择安装方案2

官方下载安装包页面：

https://github.com/tinyproxy/tinyproxy/releases

##### 1、一键安装脚本

> 以下配置，是多ip服务器（站群服务器）代理的应用场景，可自行更改相关配置。

```powershell
vim proxy.sh
```

添加内容

```powershell
#! /bin/bash

# 配置文件
CONFIG_FILE="/etc/tinyproxy/tinyproxy.conf"

# 下载
wget -P /opt/software https://github.com/tinyproxy/tinyproxy/releases/download/1.11.0-rc1/tinyproxy-1.11.0-rc1.tar.gz

# 解压
mkdir /opt/module
cd /opt/software
tar -zxvf tinyproxy-1.11.0-rc1.tar.gz -C /opt/module/

# 编译安装
cd /opt/module/tinyproxy-1.11.0-rc1/
yum -y install gcc

./configure
make
make install

# 添加配置
mkdir /etc/tinyproxy
#cp /usr/local/etc/tinyproxy/tinyproxy.conf /etc/tinyproxy/tinyproxy.conf
echo "User nobody" >> $CONFIG_FILE
echo "Group nobody" >> $CONFIG_FILE
echo "Port 8888" >> $CONFIG_FILE
echo "BindSame yes" >> $CONFIG_FILE
echo "Timeout 20" >> $CONFIG_FILE
echo "DefaultErrorFile \"/usr/local/share/tinyproxy/default.html\"" >> $CONFIG_FILE
echo "StatFile \"/usr/local/share/tinyproxy/stats.html\"" >> $CONFIG_FILE
echo "LogFile \"/var/log/tinyproxy/tinyproxy.log\"" >> $CONFIG_FILE
echo "LogLevel Info" >> $CONFIG_FILE
echo "MaxClients 500" >> $CONFIG_FILE
echo "ViaProxyName \"tinyproxy\"" >> $CONFIG_FILE
echo "BasicAuth admin 123456" >> $CONFIG_FILE

# 创建日志
mkdir /var/log/tinyproxy
touch /var/log/tinyproxy/tinyproxy.log
chmod 777 /var/log/tinyproxy/tinyproxy.log

echo '启动'
ps -ef|grep tinyproxy|grep -v grep|awk '{print "kill -9 "$2}'|sh
nohup tinyproxy -d -c /etc/tinyproxy/tinyproxy.conf > /dev/null 2>&1 &
```

添加权限

```powershell
chmod 777 proxy.sh
```

运行proxy.sh

```powershell
sh proxy.sh
```

检测

```powershell
tinyproxy -v
```

> 如果上面的一键安装配置成功运行，下面的配置参考一下就行。

##### 2、修改配置（可选）

```powershell
vim /etc/tinyproxy/tinyproxy.conf
```

主要配置内容

```powershell
# 用户和组
User nobody
Group nobody

# 监听端口
Port 8888

# 在多网卡的情况下，设置出口 IP 是否与入口 IP 相同。默认情况下是关闭的
BindSame yes

# 超时时间
Timeout 20

DefaultErrorFile "/usr/local/share/tinyproxy/default.html"

StatFile "/usr/local/share/tinyproxy/stats.html"

# 指定日志位置
LogFile "/var/log/tinyproxy/tinyproxy.log"

LogLevel Info

# 设置最大客户端链接数
MaxClients 500

ViaProxyName "tinyproxy"

# 权限校验
BasicAuth admin 123456
```

启动进程

由于正常启动会出现错误，日志不能正常输出。

```powershell
tinyproxy -c /etc/tinyproxy/tinyproxy.conf
WARNING: logging deactivated (can't log to stdout when daemonized)
```

所以，采用nohup来指定日志文件，不嫌麻烦的可以使用Supervisor来管理进程。

准备日志文件

```powershell
mkdir /var/log/tinyproxy
touch /var/log/tinyproxy/tinyproxy.log
chmod 777 /var/log/tinyproxy/tinyproxy.log
```

后台运行

```powershell
nohup tinyproxy -d -c /etc/tinyproxy/tinyproxy.conf >/var/log/tinyproxy/tinyproxy.log 2>&1 &
```

##### 3、控制程序脚本

```powershell
vim /usr/bin/tp
```

```powershell
#!/bin/bash
if [ $# -lt 1 ]
then
    echo "No Args Input..."
    exit ;
fi
case $1 in
"start")
        echo " =================== 启动 ==================="
        nohup tinyproxy -d -c /etc/tinyproxy/tinyproxy.conf > /dev/null 2>&1 &
;;
"stop")
        echo " =================== 关闭 ==================="
        ps -ef|grep tinyproxy|grep -v grep|awk '{print "kill -9 "$2}'|sh
;;
"restart")
        echo " =================== 重启 ==================="
        ps -ef|grep tinyproxy|grep -v grep|awk '{print "kill -9 "$2}'|sh
        nohup tinyproxy -d -c /etc/tinyproxy/tinyproxy.conf > /dev/null 2>&1 &
;;
"status")
        echo " =================== 状态 ==================="
        ps -ef|grep tinyproxy|grep -v grep
;;
*)
    echo "Input Args Error..."
;;
esac
```

添加权限

```powershell
chmod 777 /usr/bin/tp
```

测试脚本

```powershell
tp start
tp stop
tp status
tp restart
```

查看日志

```powershell
tail -f /var/log/tinyproxy/tinyproxy.log
```

##### 4、测试tinyproxy

```powershell
# 不加验证参数不会正常返回
curl -x http://127.0.0.1:8888 http://httpbin.org/get
Proxy Authentication Required

# 正常返回
curl -x http://admin:123456@127.0.0.1:8888 http://httpbin.org/get
```

```
注意，如果外部测试无反应，请检测是否开通服务器安全组和防火墙的8888端口。
```

iptables开放8888端口

```powershell
iptables -A INPUT -p tcp --dport 8888 -j ACCEPT
```