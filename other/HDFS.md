# HDFS常用命令
https://segmentfault.com/a/1190000002672666

# Linux_Hdfs删除空文件.md

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
### Python删除某一目录下的空文件(夹)
```
# coding: utf-8
import os  # 引入文件操作库

def CEF(path):
    """
    CLean empty files, 清理空文件夹和空文件
    :param path: 文件路径，检查此文件路径下的子文件
    :return: None
    """
    files = os.listdir(path)  # 获取路径下的子文件(夹)列表
    for file in files:
        print 'Traversal at', file
        if os.path.isdir(file):  # 如果是文件夹
            if not os.listdir(file):  # 如果子文件为空
                os.rmdir(file)  # 删除这个空文件夹
        elif os.path.isfile(file):  # 如果是文件
            if os.path.getsize(file) == 0:  # 文件大小为0
                os.remove(file)  # 删除这个文件
    print path, 'Dispose over!'

if __name__ == "__main__":  # 执行本文件则执行下述代码
    path = raw_input("Please input the files path:")  # 输入路径
    CEF(path)
```




### References
https://segmentfault.com/a/1190000002672666
