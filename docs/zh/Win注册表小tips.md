#### **Win10系统删除右键固定到"快速访问"选项的方法：**

注册表路径

```
注册表路径为
HKEY_CLASSES_ROOT\Folder\shell\pintohome
新建字符串值，然后命名为 Extended 
即可生效
```

#### Windows10系统右下角的时间显示秒钟

```

HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced
新建Dword（32位）值，然后命名为 ShowSecondsInSystemClock 
修改值为1，重启资源管理器
```

