## Debian12安装Mysql8数据库

如果是debian 11 的最小化安装，一般还需要安装 wget 、gnupg 、lsb-release

```plaintext
apt update
apt install wget gnupg lsb-release
```

具体步骤和命令

1.   下载MySQL APT 存储库， https://dev.mysql.com/downloads/repo/apt/

```plaintext
wget https://repo.mysql.com//mysql-apt-config_0.8.20-1_all.deb
```

2.   dpkg 安装存储库，并选择是否要安装配套工具。 一般选择不。 在下图中移动到ok，回车即可

```plaintext
dpkg -i mysql-apt-config_0.8.20-1_all.deb
```

如果缺少证书

```
GPG 错误：https://dl.winehq.org/wine-builds/ubuntu xenial InRelease: 由于没有公钥，无法验证下列签名： NO_PUBKEY 76F1A20FF987672F
```

去下载公钥：（需要梯子，使用proxychains下载）

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 76F1A20FF987672F
```

**PS：后面那个76F1A20FF987672F换成缺少的密钥，是什么就换成什么**

正常更新后，可能会出现Mysql安装无法继续，提示缺少核心组件，或者其他的版本冲突，

可能是因为系统版本和mysql镜像源和系统版本不匹配，查看系统版本：

```
xxx@debian:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 12 (bookworm)
Release:	12
Codename:	bookworm
```

由于当前时间新版本的系统具有新的系统版本，导航到镜像源地址：

```
sudo nano /etc/apt/sources.list.d/mysql.list
将其中的版本换成，类似：
deb [signed-by=/usr/share/keyrings/mysql-apt-config.gpg] http://repo.mysql.com/apt/debian/ bookworm mysql-8.0
修改后，应该就可以正常安装了
```

![null](https://img-blog.csdnimg.cn/60a4ede73f744eb793f637620a92e38f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGdnaXJscw==,size_20,color_FFFFFF,t_70,g_se,x_16)

3.   使用apt 命令进行安装. 如果提示出现依赖包的错误，注意分别安装。中途需设定root密码

```plaintext
apt update
apt install mysql-server
```

4.   查看 mysql 是否正常运行，检查版本号

```plaintext
systemctl status mysql
mysql --version
```

![null](https://img-blog.csdnimg.cn/e8ca4e9a4cd74911b6580fcb3a149230.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGdnaXJscw==,size_20,color_FFFFFF,t_70,g_se,x_16)

5.   进入 数据，查看用户和是否允许远程登录、连接。

首先使用 mysql_secure_installation 命令，对mysql数据库的安全性进行配置，根据需要y/n

5.1 登入数据库命令行环境

```plaintext
mysql -u root -p    #使用root账户和安装时设定的密码，进入数据库命令行模式。 如果不带-p 会报错
```

5.2 设定root账户 拥有所有数据库的权限，并且允许任意ip进行登录，也就是开启 remote

```plaintext
# 以下命令为 mysql> 命令行环境下使用
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION ;
select User, host from mysql.user;    # 查看所有用户的情况
```

注意： 在之前的mysql 版本中，还可以加上 IDENTIFIED BY 'password' 来设置密码，但新版的命令有所差别，会报错。下图中将错误的命令圈了出来了。

![null](https://img-blog.csdnimg.cn/d3686582214f4a4face75026f23a108e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGdnaXJscw==,size_20,color_FFFFFF,t_70,g_se,x_16)

 5.3.若使用 上面的过程仍提示错误：“You are not allowed to create a user with GRANT”，则进入 mysql 这个数据库中，对root 的host参数进行修改

```plaintext
#进入mysql这个数据库（以下命令仍在mysql命令行环境下进行）
use mysql;
# 直接指定root的host 参数
update user set host='%' where user='root';
#将所有数据库的权限开放给root
GRANT ALL PRIVILEGES ON *.* TO root@'%';
```

5.4. 导入你所备份的数据库文档

```plaintext
# 先进入mysql命令行模式，新建数据库。 一般保持和原有数据库的名称一致
mysql -u root -p
# 新建空数据库
mysql> create database wordpress;
mysql> quit;  # 退出数据库命令行模式

#1. 比较直接的导入方法: mysql -u username -p password database < database.back.sql
mysql -u wordpress -p 1357986420 wordpress < wordpress.back.sql   #这里用户名和数据库名相同

#2. 在数据库命令行下的导入方法
mysql -u root -p
mysql> use wordpress;   #指明要操作的数据库，并进入该数据库
mysql> set names utf8;  #设定数据库的编码（针对空数据库）
mysql> source /home/backups/wordpress.back.sql;  #导入的命令，数据库备份文档的绝对路径
mysql> quit;
```

5.5 其他的一些命令

```plaintext
# 用于显示数据库的默认字符编码的命令
mysql> show variables like '%character%';
# 用于显示所有数据库名称的命令
mysql> show databases;
# 手动编辑配置文档 
vim /etc/my.cnf
# 设置默认字符编码
[mysql]
default-character-set = utf8
[mysqld]
character-set-server = utf8
[client]
default-character-set = utf8
```

5.6 发生错误时，产生的信息文档的位置 /var/log/mysql/error.log 

6.   配置phpMyAdmin远程登录mysql 数据库。

可参考我之前的文章： PhpMyAdmin 5.04设置登录远程数据库的方法_lggirls的博客-CSDN博客 

登录截图

![null](https://img-blog.csdnimg.cn/c5efaf5071dd46f68f09ee7f801db0d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGdnaXJscw==,size_14,color_FFFFFF,t_70,g_se,x_16)

7.   安装 Oracle JDK 并设置环境变量

```plaintext
# 从官网下载最新版针对Debian 的deb包
wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.deb
apt install ./jdk-17_linux-x64_bin.deb
# 安装完成后要设置环境变量
export PATH=$PATH:/usr/lib/jvm/jdk-17/bin

# 查看一下版本
java --version
# 显示结果如下：
java 17.0.1 2021-10-19 LTS
Java(TM) SE Runtime Environment (build 17.0.1+12-LTS-39)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.1+12-LTS-39, mixed mode, sharing)
# 增加 jre home。注意:下面的命令需要设置过环境变量后，在/usr/lib/jvm/jdk-17/ 目录中执行
bin/jlink --module-path jmods --add-modules java.desktop --output jre
# 永久性增加环境变量
vim /etc/profile
# 在最下面增加如下内容，重启后生效
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_15
export PATH=$JAVA_HOME/bin:$PATH
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib
export J2SDKDIR=$JAVA_HOME/
```