## du命令

查看目录所占磁盘空间：

```
du -c -d 1  -m | sort -n
```

* -c

显示当前文件夹总计占用空间

* -d 1

层级为1，即只显示当前目录下一级文件夹的大小，不往下遍历

* -m

显示的数值以M为单位

* sort -n

结果按顺序排列
