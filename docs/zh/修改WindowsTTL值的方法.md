## windows修改TTL数值的方法

> 用[管理员](http://www.dngz.net/hot-%b9%dc%c0%ed%d4%b1.htm)权限打开命令行窗口，复制粘贴运行如下[代码](http://www.dngz.net/hot-%b4%fa%c2%eb.htm)，windows修改TTL为65：
> 
> netsh int ipv4 set glob defaultcurhoplimit=65
> netsh int ipv6 set glob defaultcurhoplimit=65
> 
> 修改完成速度恢复正常，通过Ping测试TTL可能看不出变化，实际上已经修改成功。
> 也可以看到[注册表](http://www.dngz.net/hot-%d7%a2%b2%e1%b1%ed.htm)键值已经修改了。
> 点击开始 —\> 运行，在运行对话框中输入regedit命令，弹出注册表编辑器，依次打开HKEY\_LOCAL\_MACHINE/System/CurrentControlSet/Services/Tcpip/Parameters，找到DefaultTTL，可看到DefaultTTL的键值已经修改了，也可以直接修改这个键值，[Windows系统](http://www.dngz.net/hot-windows%cf%b5%cd%b3.htm)设置后重启才生效。