# HDFS常用命令
https://segmentfault.com/a/1190000002672666


# Spark 提交到集群或者客户端
1. 集群
```
spark-submit --master yarn \
    --deploy-mode $1 \
    --driver-memory 8g \
    --executor-memory 8g \
    --num-executors 100 \
    --conf 'spark.dynamicAllocation.maxExecutors=800' \
    --conf 'spark.executor.memoryOverhead=50116' \
    --conf 'spark.speculation=true' \
    --queue maps_poi \
    --conf 'spark.speculation.quantile=0.9' \
    --conf 'spark.speculation.multiplier=1.5' \
    --archives /home/data/dongming.wang/env_python/py38_dev.tar.gz#ANACONDA\
    --conf spark.yarn.appMasterEnv.PYSPARK_PYTHON=./ANACONDA/py38_dev/bin/python \
    --conf spark.yarn.appMasterEnv.PYSPARK_DRIVER_PYTHON=./ANACONDA/py38_dev/bin/python \
    --conf spark.yarn.appMasterEnv.PYTHONPATH=/home/data/common/anaconda2/envs/py38_dev/lib/python3.8/site-packages/pyspark.zip:/home/data/common/anaconda2/envs/py38_dev/lib/python3.8/site-packages/py4j-0.10.9-src.zip \
    --conf spark.yarn.appMasterEnv.LTP_MODEL_DIR=./ANACONDA/py38_dev/lib/python3.8/site-packages \
    $2
```

2. 客户端
```
#!/bin/bash

export HDP_VERSION=3.1.4.0-315
export SPARK_YARN_USER_ENV=PYTHONHASHSEED=0
export PYSPARK_PYTHON=./py3/bin/python
export PYSPARK_DRIVER_PYTHON=/home/data/fran.yang/env_python/py36/bin/python
# export PYTHONHASHSEED=0


dict_path="/home/data/fran.yang/RecallClassifier/dict/"
files=$(ls $dict_path);
files=${files// / };
file_arr=($files);
files_str=""
for ele in ${file_arr[*]}
do
  file_str=${file_str}${dict_path}${ele},
done
len=`expr ${#file_str} - 1`
file_str=`expr substr "$file_str" 1 $len`
echo $file_str

spark-submit --master yarn \
           --queue maps_trajectory \
           --deploy-mode client\
           --executor-cores 4\
           --num-executors 500 \
           --executor-memory 24g \
           --driver-memory 32g \
           --conf spark.driver.maxResultSize=10G\
           --conf spark.speculation=true\
           --conf spark.speculation.quantile=0.9 \
           --conf spark.speculation.multiplier=1.5 \
           --conf spark.sql.hive.convertMetastoreParquet=false \
           --archives /home/data/fran.yang/env_python/py3.tar/#py3 \
           --conf spark.yarn.appMasterEnv.PYSPARK_PYTHON=./py3/bin/python \
           --py-files CleanQuery.py \
           sample.py
```


# HDFS合并小文件
1. 将本地的小文件合并，上传到HDFS   
hadoop fs -appendToFile 1.txt 2.txt hdfs://cdh5/tmp/lxw1234.txt
3. 下载HDFS的小文件到本地，合并成一个大文件   
hadoop fs -getmerge hdfs://cdh5/tmp/lxw1234/*.txt local_largefile.txt
4. 合并HDFS上的小文件  
hadoop fs -cat hdfs://cdh5/tmp/lxw1234/*.txt | hadoop fs -appendToFile - hdfs://cdh5/tmp/hdfs_largefile.txt


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
