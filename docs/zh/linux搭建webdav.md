1、安装apache

`yum install httpd* -y`

2、配置webdav

`vim /etc/httpd/conf/httpd.conf`

在最后添加

`Include conf/webdav.conf`

#指定webdav的配置文件路径

3、创建webdav配置文件

`vim /etc/httpd/conf/webdav.conf`

增加下列内容
```
&lt;IfModule mod_dav.c&gt;
LimitXMLRequestBody 131072
Alias /webdav "/var/www/webdav"
&lt;Directory /var/www/webdav&gt;
Dav On
Options +Indexes
IndexOptions FancyIndexing
AddDefaultCharset UTF-8
AuthType Basic
AuthName "WebDAV Server"
AuthUserFile /etc/httpd/webdav.users.pwd
Require valid-user
Order allow,deny
Allow from all
&lt;/Directory&gt;
&lt;/IfModule&gt;
```
4、创建访问目录

mkdir -p /var/www/webdav

chown apache:apache /var/www/webdav

5、添加用户设定密码

htpasswd -c /etc/httpd/webdav.users.pwd user01

(htpasswd -D /etc/httpd/webdav.users.pwd user01 #删除用户)

6、重启apache服务即可访问

service httpd restart