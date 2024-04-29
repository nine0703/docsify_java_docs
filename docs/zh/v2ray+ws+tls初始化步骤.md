`yum makecache && yum -y upgrade && yum -y install git curl wget nano cpp gcc tree bash-completion`

`yum -y install gcc patch libffi-devel python-devel zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel`

`yum -y install zlib*`

`yum -y install python-setuptools`

安装v2ray

`bash <(curl -s -L https://git.io/v2ray.sh)`

安装nginx

`yum install -y epel-release && yum install -y nginx`

配置nginx

Linux系统上Nginx默认站点配置文件是`/etc/nginx/conf.d/`目录下的`default.conf`，

我们对伪装网站进行全站https配置，示例内容如下：

server { listen 80; server\_name xxxxx; # 改成你的域名 rewrite ^(.*) https://$server\_name$1 permanent;}server { listen 443 ssl http2; server\_name xxxxx; charset utf-8; # ssl配置 ssl\_protocols TLSv1.2 TLSv1.3; # tls 1.3要求nginx 1.13.0及以上版本 ssl\_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384; ssl\_prefer\_server\_ciphers off; ssl\_session\_cache shared:SSL:10m; ssl\_session\_timeout 1d; ssl\_session\_tickets off; ssl\_certificate xxxxx; # 改成你的证书地址 ssl\_certificate\_key xxxx; # 改成证书密钥文件地址 access\_log /var/log/nginx/xxxx.access.log; error_log /var/log/nginx/xxx.error.log; root /usr/share/nginx/html; location / { index index.html; }}

配置好用`nginx -t`命令查看有无错误，

没问题的话`systemctl restart nginx`启动Nginx。

web服务器的根目录（默认是`/usr/share/nginx/html`）。

对于伪装站来说，静态站足够。如果你的境外流量比较大，建议用爬虫或者其他手段做一个看起来受欢迎、流量大的站点，例如美食博客，图片站等。

Nginx配置websocket

接下来我们让Nginx和v2ray结合，完成服务端的配置。首先我们选择一个**伪装路径**，

建议为二级或者较长的一级路径，例如`/abc/def`或`/awesomepath`。

配置Nginx将伪装路径的访问都转发到v2ray。

编辑`/etc/nginx/conf.d/default.conf`的第二个`server`段，增加以下转发配置：

location /awesomepath { # 与 V2Ray 配置中的 path 保持一致 proxy\_redirect off; proxy\_pass http://127.0.0.1:12345; # 假设v2ray的监听地址是12345 proxy\_http\_version 1.1; proxy\_set\_header Upgrade $http\_upgrade; proxy\_set\_header Connection "upgrade"; proxy\_set\_header Host $host; proxy\_set\_header X-Real-IP $remote\_addr; proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;}

、配置好后重启Nginx：`systemctl restart nginx`。

配置v2ray接受Nginx传来的数据。编辑 `/etc/v2ray/config.json` 文件，在“inbounds”中新增“streamSetting”配置，设置传输协议为“websocket”。配置好后`config.json`文件看起来是：{ "log": { "loglevel": "warning", "access": "/var/log/v2ray/access.log", "error": "/var/log/v2ray/error.log" }, "inbounds": \[{ "port": 12345, "protocol": "vmess", "settings": { "clients": \[ { "id": "xxxxx", # 可以使用/usr/bin/v2ray/v2ctl uuid生成 "level": 1, "alterId": 0 } \] }, "streamSettings": { # 载体配置段，设置为websocket "network": "ws", "wsSettings": { "path": "/awesomepath" # 与nginx中的路径保持一致 } }, "listen": "127.0.0.1" # 出于安全考虑，建议只接受本地链接 }\], "outbounds": \[{ "protocol": "freedom", "settings": {} },{ "protocol": "blackhole", "settings": {}, "tag": "blocked" }\], "routing": { "rules": \[ { "type": "field", "ip": \["geoip:private"\], "outboundTag": "blocked" } \] }}**注意：**1. json文件不支持注释，上述配置中”#”号及后续内容都要删掉；2. 因为使用tls加密，**强烈建议alterId改成0**，以节省cpu加快v2ray的速度

配置无误后，重启v2ray服务：`systemctl restart v2ray`。如何测试nginx与v2ray结合没有问题？打开浏览器，输入域名及其他路径，应该显示正常网页或者页面不存在，说明Nginx正常工作；输入域名加v2ray路径，例如`https://tlanyan.me/awesomepath`，应该出现”Bad Request”，说明Nginx将流量转发给了v2ray，并且v2ray收到了请求。