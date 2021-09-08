
```
JVM启动参数

目前各个应用的jvm启动参数配置五花八门，很多应用缺少一些必要的参数，导致出了故障没有日志，不容易定位问题。所以后续我们需要逐渐规范jvm的使用，现在提供一个普适的模版，标红的地方可以根据实际情况进行调整，其他参数先保持不变就行
-Xmx4g
-Xms4g
-XX:+UseG1GC
-XX:+UnlockExperimentalVMOptions
-XX:+UseCGroupMemoryLimitForHeap
-XX:MaxGCPauseMillis=200
-XX:InitiatingHeapOccupancyPercent=45
-XX:G1ReservePercent=10
-XX:+UseStringDeduplication
-XX:MetaspaceSize=256m
-XX:MaxMetaspaceSize=256m
-XX:+UseGCLogFileRotation
-XX:NumberOfGCLogFiles=10
-XX:GCLogFileSize=10M
-XX:+PrintGCDetails
-XX:+PrintGCDateStamps
-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=/home/data/logs/应用名/heapdump.hprof
-Xloggc:/home/data/logs/应用名/jvm.gc

其他环境特殊配置
1. pre环境增加 javaagent配置
  1. IAST agent接入列表域名 
```
