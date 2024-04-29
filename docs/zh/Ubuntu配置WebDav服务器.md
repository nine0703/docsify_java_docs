## [Ubuntu 配置WebDav服务器](https://www.cnblogs.com/TerryBlog/archive/2011/11/30/2269287.html)

**什么是WebDAV？**

简单地说：“基于Web的分布式创作和版本”的WebDAV。它是HTTP协议，它允许用户协作编辑和管理远程Web服务器上的文件的扩展。听说苹果的icloud 也是基于webdav来实现的,使用Ubuntu 如何配置webdav服务器呢?

首先先了解webdav的一些开源项目和商业项目,访问这个链接:http://webdav.org/.

进入终端:

步骤１:安装apache2服务:

sudo apt-get install apache2

步骤２:启用关联到的模块:

sudo a2enmod dav_fs
sudo a2enmod dav
sudo a2enmod dav_lock

步骤３:关联ＳＯ文件:

sudo ln -s /etc/apache2/mods-available/dav.load /etc/apache2/mods-enabled/dav.load
sudo ln -s /etc/apache2/mods-available/dav\_fs.load /etc/apache2/mods-enabled/dav\_fs.load
sudo ln -s /etc/apache2/mods-available/dav\_lock.load /etc/apache2/mods-enabled/dav\_lock.load
sudo ln -s /etc/apache2/mods-available/dav\_fs.conf /etc/apache2/mods-enabled/dav\_fs.conf

步骤４:重启服务:

sudo /etc/init.d/apache2 restart

步骤５:创建虚拟主机:

mkdir /var/www/sync

chown www-data:www-data /var/www/sync

步骤６:创建用户:

sudo htpasswd -c /var/www/me.dav terry
--这里会要求你重新办理确认密码
sudo chown root:www-data /var/www/me.dav

sudo chmod 640 /var/www/me.dav

步骤７:配置虚拟主机:

sudo gedit /etc/apache2/sites-available/default

在&lt;span apturetmmselection"="" style="line-height: inherit; border-collapse: collapse; clear: none; cursor: auto; margin: 0px; outline: none; vertical-align: baseline; position: relative; background-color: rgba(0, 0, 0, 0); background-image: none; border-width: 0px; display: inline; padding: 0px; color: inherit; font-family: inherit; font-size: inherit; font-style: inherit; font-variant: inherit; letter-spacing: inherit; text-transform: inherit; word-spacing: inherit; white-space: inherit;"&gt;VirtualHost 节点中加入以下配置信息:

DocumentRoot /var/www/sync/
&lt;Directory /var/www/sync/&gt;
Options Indexes MultiViews
AllowOverride None
Order allow,deny
allow from all
&lt;/Directory&gt;
Alias /webdav /var/www/sync
&lt;Location /webdav&gt;
DAV On
AuthType Basic
AuthName "webdav"
AuthUserFile /var/www/me.dav
Require valid-user

&lt;/Location&gt;

最后一步:重启服务并登录吧!

使用命令行cadaver进入登录

sudo /etc/init.d/apache2 restart

sudo apt-get install cadaver
cadaver http://127.0.0.1/webdav/

ＯＫ.