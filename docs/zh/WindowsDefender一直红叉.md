今天手动关闭Windows Defender处理了几个风险文件，结束再次打开Windows Defender后，任务栏一直显示红叉。打开后提示“发现威胁，需要采取措施。”（如图） 
![在这里插入图片描述](https://img-blog.csdnimg.cn/bd11183cab2a4455bfdb1c7f71e01de2.png#pic_center)  
诡异的是无论采取何种措施（隔离或者删除），似乎都不能生效。尽管风险文件确实已经被彻底删除了，但相关提示一直都在。

查了一圈，MS官方论坛里面修改注册表什么的都不好使。  
最后解决问题的方法是，手动删除Windows Defender的历史记录文件，路径如下：

```
C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory
```

把这个目录里的内容清空就行了。然后重启电脑，终于再次清爽了。