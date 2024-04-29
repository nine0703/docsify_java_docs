Windows Server 2019内置OpenSSH Server组件了。只不过OpenSSH Server默认是可选功能，同样需要安装才能使用。下面MS酋长就简要分享一下通过运行PowerShell命令为Windows Server 2019安装OpenSSH Server远程管理组件的方法。

右键点击开始按钮（或按Win+X组合键）弹出系统快捷菜单，选择“Windows PowerShell(管理员)”，在打开的“管理员: Windows PowerShell”窗口中输入并回车运行以下命令：

```
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

而且比较省心的是，Windows Server 2019 会自动把OpenSSH Server添加到防火墙允许列表中。

然后我们继续运行以下命令启动 sshd 和 ssh-agent 服务并设置为自动启动：

```
  Set-Service sshd -StartupType Automatic
  Set-Service ssh-agent -StartupType Automatic
  Start-Service sshd
  Start-Service ssh-agent
```

好的，现在已经成功安装OpenSSH，快连接你的服务器试试吧。