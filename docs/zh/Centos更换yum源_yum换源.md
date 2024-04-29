更换yum源的目的是，Centos本身使用的是外国yum源，当我们下载一些辅助工具的时候下载速度比较慢，所以将yum源更换为国内的yum源，提高下载速度。

## 国内的yum源

阿里：[centos7](https://so.csdn.net/so/search?q=centos7&spm=1001.2101.3001.7020)：http://mirrors.aliyun.com/repo/Centos-7.repo
网易：centos7：http://mirrors.163.com/.help/CentOS7-Base-163.repo

## 操作步骤（网易）

1、yum源进行备份
进入到yum源的配置文件中
执行命令如下：cd /etc/[yum](https://so.csdn.net/so/search?q=yum&spm=1001.2101.3001.7020).repos.d
将yum源进行备份：mv Centos-Base.repo Centos-Base.repo.bak
2、下载网易yum源
执行命令：wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
3、更换yum源
执行命令：mv CentOS7-Base.repo Centos-Base.repo
4、生成yum缓存
执行命令：yum makecache
5、对yum源进行更新
执行命令：yum -y update
上述就是更改yum源的步骤，更新yum源过程需要等待一段时间，等yum源更新完成之后，yum源就可以使用。

## 操作步骤(阿里)

1、yum源进行备份
进入到yum源的配置文件中
执行命令如下：cd /etc/yum.repos.d
将yum源进行备份：mv Centos-Base.repo Centos-Base.repo.bak
2、获取阿里的yum源配置文件
执行命令：wget -O Centos-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
3、对yum源生成缓存
执行命令：yum makecache
4、更新yum源
执行命令：yum -y install update
执行完成之后就可以使用yum源了，到此yum源就更换成功了。

CREATE USER joplin_user WITH
CREATEDB
CREATEROLE
PASSWORD 'rcijoplin';