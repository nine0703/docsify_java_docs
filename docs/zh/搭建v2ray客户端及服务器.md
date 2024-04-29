## 搭建v2ray客户端及服务器
- **官网**：`https://www.v2ray.com`

> V2Ray(Project V) 相对于 Shadowsocks V2Ray 更像全能选手，拥有更多可选择的协议 / 传输载体 (Socks、HTTP、TLS、TCP、mKCP、WebSocket )，还有强大的路由功能，不仅仅于此，它亦包含 Shadowsocks 组件，你只需要安装 V2Ray，你就可以使用所有的 V2Ray 相关的特性包括使用 Shadowsocks，由于 V2Ray 是使用 GO 语言所撰写的，天生的平台部署优势，下载即可使用，当然啦，由于 V2Ray 的配置相对来说是很繁琐的，毫无夸张的说，但是有了本人所写的 V2Ray 一键安装脚本 加持下，使用 V2Ray 便会显得轻松多了。

### 安装 V2Ray服务端

    Xshell 界面按 Shift + Insert 即可粘贴，不能按 Ctrl + V

`bash <(curl -s -L https://git.io/v2ray.sh)`

然后选择安装，即是输入 1 回车

选择传输协议，如果没有特别的需求，使用默认的 TCP 传输协议即可，直接回车

选择端口，如果没有特别的需求，使用默认的端口即可，直接回车

是否屏蔽广告，除非你真的需要，一般来说，直接回车即可

是否配置 Shadowsocks ，如果不需要就直接回车，否则就输入 Y 回车

Shadowsocks 端口，密码，加密方式这些东西自己看情况配置即可，我个人当然是全部直接回车。。

OK，按回车继续

- **安装信息** 如果确保没有什么问题了，按回车继续

- **备注** 安装信息会因你的配置而变化
  
- **V2Ray 安装完成** 如上图所示，V2Ray 配置信息，Shadowsocks 配置信息都有了
    如果你使用过 Shadowsocks ，那么现在你可以测试一下 Shadowsocks 配置了，看看是否能正常使用。
    如果你使用过 V2Ray 某些客户端，那么现在也可以测试一下配置了。
    (备注，可能某些 V2Ray 客户端的选项或描述略有不同，但事实上，上面的 V2Ray 配置信息已经足够详细，由于客户端的不同，请对号入座。)
    
- **V2Ray 管理面板** 现在可以尝试一下输入 `v2ray` 回车，即可管理 V2Ray[](https://www.pohaier.com/usr/uploads/2020/03/3872615350.jpg)
  
- **TCP 阻断** 如果你觉得你的小鸡出现了这种情况，那么可以尝试使用 UDP 协议相关的 mKCP
    当然，用了我的脚本那是很简单的啦，直接输入 v2ray config 然后选择修改 V2Ray 传输协议
    之后再选择 mKCP 相关的就行咯
    备注：使用 mKCP 或许还可以提高速度，但由于 UDP 的原因也许会被运营商 Qos，这是无解的。
    
### 防火墙配置

| 功能                            | 命令                                             |
| ------------------------------- | ------------------------------------------------ |
| 查看防火墙是否开启              | systemctl status firewalld                       |
| 开启防火墙（若未开启）          | systemctl start firewalld                        |
| 查看所有开启的端口              | firewall-cmd --list-ports                        |
| 永久添加33851端口               | firewall-cmd --add-port=33851/tcp --permanent    |
| 永久移除22851端口               | firewall-cmd --remove-port=33851/tcp --permanent |
| 重载防火墙配置                  | firewall-cmd --reload                            |
| 查看已开放的端口                | firewall-cmd --zone=public --list-ports          |
| 查看 V2Ray 配置信息             | v2ray info                                       |
| 修改 V2Ray 配置                 | v2ray config                                     |
| 生成 V2Ray 配置文件链接         | v2ray link                                       |
| 生成 V2Ray 配置信息链接         | v2ray infolink                                   |
| 生成 V2Ray 配置二维码链接       | v2ray qr                                         |
| 修改 Shadowsocks 配置           | v2ray ss                                         |
| 查看 Shadowsocks 配置信息       | v2ray ssinfo                                     |
| 生成 Shadowsocks 配置二维码链接 | v2ray ssqr                                       |
| 查看 V2Ray 运行状态             | v2ray status                                     |
| 启动 V2Ray                      | v2ray start                                      |
| 停止 V2Ray                      | v2ray stop                                       |
| 重启 V2Ray                      | v2ray restart                                    |
| 查看 V2Ray 运行日志             | v2ray log                                        |
| 更新 V2Ray                      | v2ray update                                     |
| 更新 V2Ray 管理脚本             | v2ray update.sh                                  |
| 卸载 V2Ray                      | v2ray uninstall                                  |

> ==如果v2ray失效，可以尝试更新客户端和服务端的v2ray内核==

* * *

## V2ray客户端

### 官网下载

```
https://www.v2ray.com/  
```

### 配置信息

懒得写了，很简单



==2022.03.05 22:23==