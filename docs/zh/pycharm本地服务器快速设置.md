## Code With Me 快速设置

快速Code With Me设置旨在用于快速评估目的。此设置具有以下与安全相关的限制：

- 该设置没有用于大厅或中继服务器连接的 SSL。
- 中继服务器无法验证建立中继的请求是否真实，是否来自真实的大厅服务器。
- 中继服务器或主机无法验证会话加入请求是否由大厅服务器签名。

查看以下视频，了解如何快速配置本地设置：

### 设置启动器

- 点击此[链接获取设置启动器](https://www.jetbrains.com/code-with-me/on-prem/#downloads)。

  快速设置使用Code With Me服务器的单击可执行文件。

  > 服务器只能在`linux-x64`平台上启动。

### 发射

您可以在代码中使用以下选项来启动Code With Me会话的服务器：

- 主机地址由选项指定`-h`。

- 许可证文件的路径由该选项指定`-k`。要获取许可证文件，请访问[我们的网站](https://www.jetbrains.com/code-with-me/on-prem/)

- `-l`可以分别使用和选项指定大厅和中继服务器端口`-r`。

  服务器默认为`2093`大厅服务器和`3274`中继服务器。

```
输入以下命令：

$./launch.sh -h 10.2.2.53 -k license.key

此命令启动大厅服务器，可通过地址 http://10.2.2.53:2093访问。

该命令还会在端口上启动一个中继服务器3274，大厅服务器将共享一个ws: //10.2.2.53 :3274的链接作为中继地址。
```

方法2

```
输入以下命令：

$./launch.sh -h myserver.internal -k license.key -l 4950 -r 8092

此命令启动大厅服务器，可通过地址 http: //myserver.internal :4950访问。

此命令还会在端口上启动一个中继服务器8092，大厅将共享一个ws: //myserver.internal :8092的链接作为中继地址。
```

## 用于快速设置的离线模式设置示例

您可以在不连接到 JetBrains 站点的情况下设置 Code With Me。默认情况下，快速设置脚本不包含此类设置。简而言之，场景如下：

### 离线模式下的快速设置测试

1. 从具有 Internet 连接的计算机上的 JetBrains 站点下载所需文件，
2. 将它们放置到最终用户工作站可用的本地存储中，
3. 指向大厅服务器使用这个地方作为[本地存储](https://www.jetbrains.com/help/cwm/guest-local-storage-setup.html)。

详情请参考以下说明：

### 调整快速设置启动器

1. 在可以访问 Internet 的机器上的某处，运行一个单独的“mirror_guests”脚本 ( `lobby/bin/mirror-guests`) 和所需的过滤器。

   注意：自版本 1698 起，“mirror-guests”脚本已重命名为[page](https://www.jetbrains.com/code-with-me/on-prem/)`jetbrains-clients-downloader`并作为单独的分发版提供。

   过滤器作为参数出现并描述将下载哪些文件。使用以下示例：

   `.bin/jetbrains-clients-downloader --verbose --products-filter IU --platforms-filter windows-x64 --build-filter 211.7142.45 /tmp/CWMsources`

   211.7142.45 和 /tmp/CWMsources 是示例，相应地调整这些代码部分。有关详细语法，请参阅`jetbrains-clients-downloader -h`。

2. 将保存的文件放在没有 Internet 连接的机器上，并将测试快速设置。包含文件的文件夹将被视为`GUESTS_LOCAL_STORAGE_DIRECTORY`.

3. 将以下条目添加到`launch.sh`脚本中：

   ```none
   export GUESTS_LOCAL_STORAGE=true
   export GUESTS_LOCAL_STORAGE_DIRECTORY=
   ```

   

   的值`GUESTS_LOCAL_STORAGE_DIRECTORY=`等于上一步的位置。