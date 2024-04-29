#### 建立自己的APT源

建立自己的APT源可以让您在本地或私有网络中管理和分发软件包。这对于内部软件分发、定制软件包、加快软件包下载速度等情况非常有用。以下是在Ubuntu或Debian系统上建立自己的APT源的一般步骤：

1. 安装必要的软件包：

首先，确保您的系统已经安装了以下软件包：`apt-transport-https`、`dpkg-dev`、`gnupg`。如果缺少其中的任何一个，请使用以下命令安装它们：

```
sudo apt update
sudo apt install apt-transport-https dpkg-dev gnupg
```

1. 创建APT源目录：

在您的系统上选择一个目录来存储APT源。通常，这个目录位于Web服务器的根目录下，例如`/var/www/html`。在该目录下创建几个子目录来存储不同版本的软件包，例如`stable`、`testing`、`unstable`等。

```
sudo mkdir -p /var/www/html/apt/stable
sudo mkdir -p /var/www/html/apt/testing
sudo mkdir -p /var/www/html/apt/unstable
```

1. 复制软件包到APT源目录：

将您自己的软件包（deb文件）复制到相应的APT源目录中。您可以手动复制，或者使用`cp`命令。

```
sudo cp your-package.deb /var/www/html/apt/stable
```

1. 生成APT索引：

在APT源目录中生成APT索引文件。索引文件告诉APT软件包的位置和元数据。

```
cd /var/www/html/apt/stable
sudo dpkg-scanpackages . /dev/null | gzip -9c > Packages.gz
```

1. 添加GPG密钥：

为了确保软件包的完整性和安全性，您可以使用GPG密钥来签名APT索引和软件包。首先，生成GPG密钥对：

```
gpg --gen-key
```

根据提示生成GPG密钥对。然后，导出公钥并复制到APT源目录：

```
gpg --armor --export your@email.com > /var/www/html/apt/stable/public.key
```

1. 配置APT源：

在客户端系统上，您需要将您的APT源配置添加到`/etc/apt/sources.list`文件中。假设您的APT源地址是`http://your-server/apt/stable`，则在客户端上添加以下行：

```
deb http://your-server/apt/stable ./ # 注意：末尾的"./"表示搜索当前目录
```

1. 导入GPG密钥：

在客户端上导入您的GPG公钥，以便验证软件包的签名：

```
wget -qO - http://your-server/apt/stable/public.key | sudo apt-key add -
```

1. 更新APT缓存：

最后，更新APT缓存并开始使用您自己的APT源：

```
sudo apt update
```

现在，您已经成功建立了自己的APT源，并可以使用它来安装和更新软件包。请注意，建立APT源需要一些系统管理和网络知识，因此请谨慎操作，并确保安全性和合法性。在生产环境中，建议使用HTTPS来保护APT源的传输安全。

#### 充实自己的APT源

在服务器上配置好公开源之后，可以使用

```
apt download example-package
```

下载对应的软件包及其依赖

**保存软件包到本地仓库**：

将下载的软件包移动到您APT服务器的本地仓库目录。通常，本地仓库目录位于 

```
/var/cache/apt/archives/
```

**将软件包移动到本地仓库目录**

将您自己的软件包（deb文件）复制到相应的APT源目录中。您可以手动复制，或者使用`cp`命令。

```
sudo cp /var/cache/apt/archives/your-package.deb /var/www/html/apt/stable
```

然后正常生成索引就好了