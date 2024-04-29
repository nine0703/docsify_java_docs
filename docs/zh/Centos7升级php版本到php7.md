* * *

### 一、首先查看是否有老版本

`yum list installed | grep php`

### 二、如果安装的有

`yum remove php.x86_64 php-cli.x86_64 php-common.x86_64 php-gd.x86_64 php-ldap.x86_64 php-mbstring.x86_64 php-mcrypt.x86_64 php-mysql.x86_64 php-pdo.x86_64`

### 三、老版本清理干净之后，进行升级

### 1、由于linux的yum源不存在php7.x，所以我们要更改yum源：

`rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`

`rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`

### 2、查看yum源中有没有php7.xyum search php7看到下图，证明php已经存在yum源中

### 3、yum 安装php72w和各种拓展，选自己需要的即可

`yum -y install php72w php72w-cli php72w-common php72w-devel php72w-embedded php72w-fpm php72w-gd php72w-mbstring php72w-mysqlnd php72w-opcache php72w-pdo php72w-xml`

> ### **安装完成**

### 4、查看php版本

`php -v`

* * *

==2022.03.05 21:50==