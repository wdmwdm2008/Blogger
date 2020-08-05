需要分清三个概念：

物理CPU

物理CPU的核数

物理CPU的核是否支持超线程

总核数 = 物理CPU个数 X 每颗物理CPU的核数  
总逻辑CPU数 = 物理CPU个数 X 每颗物理CPU的核数 X 超线程数  
 
查看物理CPU个数
cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l
 
查看每个物理CPU中core的个数(即核数)
cat /proc/cpuinfo | grep "cpu cores" | uniq
 
查看逻辑CPU的个数
cat /proc/cpuinfo | grep "processor" | wc -l


单独查看内存使用情况的命令：free -m  
查看内存及cpu使用情况的命令：top  
也可以安装htop工具，这样更直观，  
安装命令如下：sudo apt-get install htop  
安装完后，直接输入命令：htop  
就可以看到内存或cpu的使用情况了。  





### Reference
https://blog.csdn.net/vola9527/article/details/85237369
