### Linux 中Python删除某根目录下的所有空目录  
```
>>> import os
>>> for root, dirs, files in os.walk("D:\data"):
...     if not os.listdir(root):
...         os.rmdir(root)
...
>>> os.listdir("D:\data")
['data1', 'data2']
```
### Hadoop HDFS 0字节空文件清理
```
hadoop fs -ls  -R /cleandata |grep "gz"|awk '{ if ($5 == 0) print $8 }'|xargs hadoop fs -rm
hadoop fs -ls  -R /cleandata |grep "gz"|awk '{ if ($5 == 20) print $8 }'|xargs hadoop fs -rm


其中，需要自定义hdfs目录
     $5 是列出的文件大小,单位字节
     $8 是列出的文件路径
     如果不使用grep,ls命令检索结果会包含目录,所以-rm清理的时候会报错提示是目录而无法删除目录，变相实现了删除文件的方案(切记rm不要使用-r参数)

```
