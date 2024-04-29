一、yum 源准备

1、先更新一下yum：

`yum -y update`

该 -y 标志用于提醒系统我们知道我们正在进行更改，免去终端提示我们要确认再继续

2、安装yum-utils 【一组扩展和补充yum的实用程序和插件】

`yum -y install yum-utils`

3、安装CentOS开发工具 【用于允许您从源代码构建和编译软件】

`yum -y groupinstall development`

二、安装Python3

1、安装EPEL：

`yum -y install epel-release`

2、安装IUS软件源：

`yum -y install https://repo.ius.io/ius-release-el7.rpm`

3、安装Python3.6：

`yum -y install python36u`

4、安装pip3：

`sudo yum -y install python36u-pip`

5、检查一下安装情况，分别执行命令查看：

先执行：

`python3 -V`

pip3 -V 查看python3 的安装情况

如果成功显示出 Python 3.6.8 则代表安装成功了，无需再操作

如果未显示版本就接着往下执行：

```
python3.6 -V
pip3.6 -V
```

在 /usr/lib/目录下可以看到Python3.6的文件夹

添加软链接

使用python3去使用Python3.6：

`ln -s /usr/bin/python3.6 /usr/bin/python3`

复制代码pip3.6同理：

`ln -s /usr/bin/pip3.6 /usr/bin/pip3`

我们可以看到，软链接是创建成功了的。

此时，再去使用python3、pip3就会是成功的