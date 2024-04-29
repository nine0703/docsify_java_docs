## CentOS7 安装 TigerVNC Server

在 CentOS 中，前提需要安装好 X-Windows。

##### 安装  

`yum install tigervnc-server -y`  

##### 配置  
```
# 安装
yum install tigervnc-server -y

# 配置
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
$ vim /etc/systemd/system/vncserver@:1.service
..
# Quick HowTo:
# 1. Copy this file to /etc/systemd/system/vncserver@.service
# 2. Replace <USER> with the actual user name and edit vncserver
#    parameters in the wrapper script located in /usr/bin/vncserver_wrapper
# 3. Run `systemctl daemon-reload`
# 4. Run `systemctl enable vncserver@:<display>.service`
...
# Root 用户
ExecStart=/usr/bin/vncserver_wrapper root %i

# 设置 VNC 登录密码
vncpasswd

# 启动服务
systemctl daemon-reload
systemctl start vncserver@:1.service && systemctl enable vncserver@:1.service && systemctl status vncserver@:1.service

# 查看 VNC 端口，默认为 5900，会自增 1。
netstat -lnpt | grep Xvnc

-----------------------------------
©著作权归作者所有：来自51CTO博客作者云物互联的原创作品，请联系作者获取转载授权，否则将追究法律责任
Linux 安装 TigerVNC
https://blog.51cto.com/u_15301988/5110215
```



添加下列行到覆盖原来的vncserver@:1.service.  注意：下面的两处xxx替换为自己的而用户名

```
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target
[Service]
Type=forking
User=xxx
# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=-/usr/bin/vncserver -kill %i
ExecStart=/usr/bin/vncserver %iPIDFile=/home/xxx/.vnc/%H%i.pid
ExecStop=-/usr/bin/vncserver -kill %i
[Install]
WantedBy=multi-user.target
```

启动命令

```
systemctl daemon-reload

systemctl start vncserver@:1

systemctl status vncserver@:1

systemctl enable vncserver@:1
```

痛哭流涕，完全成功不了，别信这吊毛教程，艹
