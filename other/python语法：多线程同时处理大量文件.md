### 按照顺序读取文件（e.g. 0.txt, 1.txt, 2.txt）
```
import operator  
import csv  
import os  
def read_file(filpos,i):  
    with open(filpos+str(i)+".csv") as f:  
        reader=csv.reader(f)  
        for i in reader:  
            print(i)  
 
for i in range(0,3):  
    read=read_file("D:/zhihu/",i)  
```

### 使用多线程读取
```
import operator
import csv
import time
import threading
from time import ctime
 
def read_file(filpos,i):
    with open(filpos+str(i)+".csv") as f:
        reader=csv.reader(f)
        for i in reader:
            print(i)
 
threads = []
x=0
for t in range(0,3):
    t= threading.Thread(target=read_file,args=("D:/zhihu/",x))
    threads.append(t)
    x+=1
#join在里面时候只有第一个子进程结束才能打开第二个进程,if__name__ 调用时不可用
if __name__=="__main__":
    for thr in threads:
        thr.start()
    thr.join()
    print("all over %s"%ctime())
```

### 使用进程读取
```
# coding:utf-8
import os
import multiprocessing as mp

filename = "www.geniatech.net"
cores = 5


def process_wrapper(chunkStart, chunkSize):
    num = 0
    with open(filename) as f:
        f.seek(chunkStart)
        lines = f.read(chunkSize).splitlines()
        for line in lines:
            num +=1
    return num

def chunkify(fname,size=1024*1024):
    fileEnd = os.path.getsize(fname)
    with open(fname,'r') as f:
        chunkEnd = f.tell()
        while True:
            chunkStart = chunkEnd
            f.seek(size,1)
            f.readline()
            chunkEnd = f.tell()
            yield chunkStart, chunkEnd - chunkStart
            if chunkEnd > fileEnd:
                break

pool = mp.Pool(cores)
jobs = []

for chunkStart, chunkSize in chunkify(filename):
    jobs.append(pool.apply_async(process_wrapper, (chunkStart,chunkSize)))

res = []
for job in jobs:
    res.append(job.get())

pool.close()
print sum(res)
```

```
from multiprocessing import Process

processes = []
for file in file_lists:
    processes.append(Process(target=read_file,args=(os.path.join('data',file),)))

for process in processes:
    process.start()

for process in processes:  
    process.join()
```
