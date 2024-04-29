WINDOW系统

```
sc.exe config lanmanserver start= delayed-auto

sc.exe config iphlpsvc start= auto

sc.exe config lanmanserver depend= SamSS/Srv2/iphlpsvc 

netsh interface portproxy add v4tov4 listenaddress=10.255.255.1 listenport=445 connectaddress=199.255.97.55 connectport=8445 

netsh interface portproxy add v4tov4 listenaddress=10.255.255.1 listenport=139 connectaddress=199.255.97.55 connectport=8139

netsh interface portproxy add v4tov4 listenaddress=10.255.255.1 listenport=137 connectaddress=199.255.97.55 connectport=8137

netsh interface portproxy add v4tov4 listenaddress=10.255.255.1 listenport=138 connectaddress=199.255.97.55 connectport=8138
```

LINUX系统

```
mount -t cifs -o username=test,password=123456 //199.255.97.55/mnt/user/Filestation /mnt/local2

将挂载命令写入：/etc/fstab

//hostname/share /localfloder cifs default,username=用户名,password=密码
先安装
apt-get install smbfs

smbmount //hostname/directory /home/my/mount/dir -o username=username,pass=passpord,iocharset=utf8

在上面的命令里

//hostname/directory 就是windows的共享路径，hostname为其IP

/home/my/mount/dir 就是本地的挂载目录，
```