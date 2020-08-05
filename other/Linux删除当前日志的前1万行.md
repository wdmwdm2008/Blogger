### 删除一个日志的前1000000行日志。
```
[root@arpapp primetongw]# cat nohup.out |wc -l
5695412
[root@arpapp primetongw]# sed -i '1,1000000d' nohup.out （d命令的意思是删除）
[root@arpapp primetongw]# cat nohup.out |wc -l
4695462
```
