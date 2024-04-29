## [Linux 文件名乱码的文件无法删除](https://www.shuzhiduo.com/A/A7zgW6Znz4/)

可以通过ls -i命令获得文件的节点号 

```
ls -i
```

通过节点号删除 find -inum 节点号 -delete

```
find -inum 节点号 -delete
```

如果是文件夹，可以使用find命令查找然后cd

```
cd 
```

