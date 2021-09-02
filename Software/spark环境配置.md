### 03_spark环境有关

```
Mac os（10.15.7）安装spark
(可选安装)
1.安装brew（mac包管理工具）
https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/
或
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
          
          

          
2.安装java
brew cask install adoptopenjdk/openjdk/adoptopenjdk8
(brew install --cask adoptopenjdk/openjdk/adoptopenjdk8)

          
export JAVA_HOME='/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home'
source ~/.zshrc

          

          
3.安装python(省略)

          

          
4.配置ssh免密登陆
对于mac用户，需要打开远程登陆。






ssh-keygen -t rsa -P ""
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

          

          
执行ssh localhost后，如果不用输入密码即可。

          

          
5.安装hadoop

          

          
下载http://mirror.bit.edu.cn/apache/hadoop/common/
我选择了hadoop-3.1.4.tar.gz下载

          

          
解压后移动
mv hadoop-3.1.4 /usr/local/Cellar/.

          

          
Hadoop 解压后即可使用。输入如下命令来检查 Hadoop 是否可用，成功则会显示 Hadoop 版本信[息：
cd /usr/local/Cellar/hadoop-3.1.4
./bin/hadoop version

          

          
紧接着就是修改有关配置文件：
（1）hadoop-env.sh
vi /usr/local/Cellar/hadoop-3.1.4/etc/hadoop/hadoop-env.sh

          

          
export JAVA_HOME='/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home'
export HADOOP_HOME='/usr/local/Cellar/hadoop-3.1.4'
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/Hadoop
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true -Dsun.security.krb5.debug=true -Dsun.security.spnego.debug"

          

          
（2）core-site.xml 
设置 Hadoop 的临时目录和文件系统，localhost:9000 表示本地主机。如果使用远程主机，要用相应的 IP 地址来代替，填写远程主机的域名，则需要到 /etc/hosts 文件中做 DNS 映射。

          

          
vi /usr/local/Cellar/hadoop-3.1.4/etc/hadoop/core-site.xml

          

          
<configuration>
  <property>
<!--用来指定hadoop运行时产生文件的存放目录  自己创建-->
  <name>hadoop.tmp.dir</name>
  <value>/usr/local/Cellar/hadoop-3.1.4/tmp</value>
  <description>Abase for other temporary directories.</description>
  </property>
  <property>
  <name>fs.defaultFS</name>
  <value>hdfs://localhost:9000</value>
  </property>
</configuration>

          

          
（3）hdfs-site.xml
注意 name 和 data 的路径都要替换成本地的路径。
vi /usr/local/Cellar/hadoop-3.1.4/etc/hadoop/hdfs-site.xml

          

          
<configuration>
  <property>
  <name>dfs.replication</name>
  <value>1</value>
  </property>
  <property>
  <name>dfs.namenode.name.dir</name>
  <value>/usr/local/Cellar/hadoop-3.1.4/tmp/dfs/name</value>
  </property>
  <property>
  <name>dfs.datanode.data.dir</name>
  <value>/usr/local/Cellar/hadoop-3.1.4/tmp/dfs/data</value>
  </property>
</configuration>

          

          
（4）mapred-site.xml
如果对于目录没有该文件，可以把mapred-site.xml.template 拷贝为 mapred-site.xml。

          

          
vi /usr/local/Cellar/hadoop-3.1.4/etc/hadoop/mapred-site.xml
<configuration>
  <property>
  <name>mapred.job.tracker</name>
  <value>localhost:9010</value>
  </property>
  <property>
  <!--指定mapreduce运行在yarn上-->
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
  </property>
</configuration>

          

          
（5）yarn-site.xml
配置数据的处理框架 yarn。
vi /usr/local/Cellar/hadoop-3.1.4/etc/hadoop/yarn-site.xml
<configuration>

          

          
<!-- Site specific YARN configuration properties -->

          

          
  <property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
  </property>

          

          
</configuration>

          

          
（6）配置环境变量

          

          
export HADOOP_HOME=/usr/local/Cellar/hadoop-3.1.4
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$JAVA_HOME/bin:$PATH

          

          
source ~/.zshrc

          
          

          
（7）运行查看
格式化文件系统（是对namenode进行初始化）：
hdfs namenode -format



启动NameNode和DataNode：
start-dfs.sh



如果报错 "connection refused"，则需要在计算机系统设置中打开远程登录许可。 






启动 yarn
start-yarn.sh



最后执行：
jps
结果如下，证明 Hadoop 可以成功启动：



打开网页http://localhost:9870




          
          

          
6.安装spark
下载https://mirrors.tuna.tsinghua.edu.cn/apache/spark/

          

          
我选择的下载链接：
https://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-3.0.1/spark-3.0.1-bin-without-hadoop.tgz
https://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7-hive1.2.tgz（支持hive的spark）

          

          
解压后移动到/usr/local/Cellar/spark-3.0.1-bin-without-hadoop
/usr/local/Cellar/spark-3.0.1-bin-hadoop2.7-hive1.2：

          

          
cd /usr/local/Cellar/spark-3.0.1-bin-without-hadoop
cd /usr/local/Cellar/spark-3.0.1-bin-hadoop2.7-hive1.2

          

          
cp ./conf/spark-env.sh.template ./conf/spark-env.sh

          

          
（1）spark-env.sh
编辑spark-env.sh文件，并添加以下配置信息:
export SPARK_DIST_CLASSPATH=$(/usr/local/Cellar/hadoop-3.1.4/bin/hadoop classpath)

          

          
有了上面的配置信息以后，Spark就可以把数据存储到Hadoop分布式文件系统HDFS中，也可以从HDFS中读取数据。如果没有配置上面信息，Spark就只能读写本地数据，无法读写HDFS数据。

          

          
（2）修改环境变量
export SPARK_HOME=/usr/local/Cellar/spark-3.0.1-bin-without-hadoop
export SPARK_HOME=/usr/local/Cellar/spark-3.0.1-bin-hadoop2.7-hive1.2
export PATH=$SPARK_HOME/bin:$PATH
export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.9-src.zip:$PYTHONPATH
export PYSPARK_PYTHON=python3

          

          
source ~/.zshrc
需要注意，上面的配置项中，PYTHONPATH这一行有个py4j-0.10.9-src.zip，这个zip文件的版本号一定要和"/usr/local/Cellar/spark-3.0.1-bin-without-hadoop/python/lib/usr/local/Cellar/spark-3.0.1-bin-hadoop2.7-hive1.2"目录下的py4j-0.10.9-src.zip文件保持版本一致。

          

          
然后把/usr/local/Cellar/spark-3.0.1-bin-without-hadoop/python/lib/usr/local/Cellar/spark-3.0.1-bin-hadoop2.7-hive1.2下面的pyspark.zip和py4j-0.10.9-src.zip移动至/Users/lixiangge/anaconda3/lib/python3.8/site-packages并用unzip解压。
其中pyspark.zip解压后，直接可以当python的pyspark包用。

          

          
建议上面2步都执行，特别是最后一步。

          

          
（3）验证安装是否成功
通过运行Spark自带的示例，验证Spark是否安装成功。
run-example SparkPi 2>&1 | grep "Pi is"




          

          
spark-shell




          

          
下面这个目录是大数据环境（开发机）对应spark的安装目录：
cd /usr/hdp/3.1.4.0-315/spark2/bin

          

          
7.安装hive

          

          
下载https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-3.1.2/，解压后移动到
/usr/local/Cellar/apache-hive-3.1.2-bin：
（1）guava替换
对比下hadoop安装目录下的/usr/local/Cellar/hadoop-3.1.4/share/hadoop/common/lib/guava-27.0-jre.jar
与hive目录下面的/usr/local/Cellar/apache-hive-3.1.2-bin/lib下面的guava版本，如果两者不一致，删除版本低的，
并拷贝高版本。
如果按照前面介绍的步骤安装，hive对于的版本较低，需要进行替换。
（2）修改环境变量
export HIVE_HOME=l
export PATH=$HIVE_HOME/bin:$PATH
source ~/.zshrc

          

          
8.Jupyter Notebook调试PySpark程序
（1）首先anaconda已经安装
已经在hwy-hn1-bigdata-hdp-map-client-prd-01（172.19.104.80）的/home/data/common/anaconda2目录下面。
已经和大数据环境运维的同事沟通过，目前连接的hwy-hn1-bigdata-hdp-map-client-prd-01（172.19.104.80）
只能使用py2环境（配置py3后，服务器已有的spark不能用，如果自己重新配spark环境说不定可以，毕竟前面配置的本地mac环境就是py3）。
目前已经在/home/data/commom目录下配置了py2的环境，可以自行修改配置文件。
export SPARK_HOME=/usr/hdp/3.1.4.0-315/spark2
export PYSPARK_PYTHON=python2
export PYSPARK_DRIVER_PYTHON=python2
export PATH="/home/data/common/anaconda2/bin:$PATH"

          
（2）jupyter notebook --generate-config



（3）终端执行python
>>>from notebook.auth import passwd
>>>passwd()



输入2次密码，用于后面登陆jupyter验证。
然后把'sha1:f3e5ea2b7b72:58276bfac2c43e2fde1cc3a0832a55a32803289e'复制，用于后面文件配置。

          

          
（4）vi /Users/lixiangge/.jupyter/jupyter_notebook_config.py(修改的是第二步生成的文件路径)
c.NotebookApp.ip='*' # 就是设置所有ip皆可访问
py2可能需要换成(c.NotebookApp.ip='0.0.0.0')
c.NotebookApp.password = 'sha1:f3e5ea2b7b72:58276bfac2c43e2fde1cc3a0832a55a32803289e' # 上面复制的那
个sha密文'
c.NotebookApp.open_browser = False   # 禁止自动打开浏览器
c.NotebookApp.port = 8888 # 端口
c.NotebookApp.notebook_dir = '/Users/lixiangge/Downloads/jupyternotebook'  #设置Notebook启动进入的目录
/.bash
需要注意启动的目录一定先建好。

          

          
（5）jupyter notebook
 http://localhost:8888/login?next=%2Ftree%3F






此时，本地就可以调试。

          
在hwy-hn1-bigdata-hdp-map-client-prd-01启动jupyter，由于目前只开放了8088端口，因此启动时可以使用 jupyter notebook --port=8088，



必须连接office网，该网可以ping通跳板机（ALY-HN1-Bigdata-Jumper-PRD-01  172.18.37.231）和大数据开发环境（hwy-hn1-bigdata-hdp-map-client-prd-01  172.19.104.80），所以就很方便连接。
访问http://172.19.104.80:8088/，然后输入之前配置的密码即可使用。




          

          
（6）补充
也可以换成jupyterlab；同时他们也有不少插件（多行输出、代码提示等），大家可以自行搜索安装。
多行输出：
vi /Users/lixiangge/.ipython/profile_default/ipython_config.py
c = get_config()
c.InteractiveShell.ast_node_interactivity = "all"
保存上面2行。



代码提示：

          

          
如果没有安装以下2个包，则先需要安装
安装jupyter_contrib_nbextensions
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user

          

          
安装nbextensions_configurator
pip install jupyter_nbextensions_configurator
jupyter nbextensions_configurator enable --user




          
          

          
重启jupyter后，在弹出的主页面里，能看到增加了一个Nbextensions标签页，在这个页面里，勾选Hinterland即可启用代码自动补全。

          

          
9.开发机jupyter使用py3+spark
base的虚拟环境是python2.7



py36_dev的虚拟环境是python3.6



jupyter的kernel选择对应的虚拟环境（这里选择的是py36_dev），然后按照以下代码进行测试。

Import os
import numpy
from pyspark.sql import SparkSession, Row
from pyspark.sql.functions import udf, col, explode
from pyspark.sql.types import StringType
 
os.environ['PYSPARK_DRIVER_PYTHON'] = "/home/data/common/anaconda2/envs/py36_dev/bin/python"
os.environ['PYSPARK_PYTHON'] = '/home/data/common/anaconda2/envs/py36_dev/bin/python'

def f(x):
return numpy.__version__ + "##!"

spark = SparkSession \
          .builder \
          .master("local[50]") \
          .appName("JB Test") \
          .enableHiveSupport() \
          .getOrCreate()
 
str(spark.sparkContext.parallelize(range(5)).map(f).distinct().collect())
df = spark.sql(
          "select id, name, address from lbs.order_poi_fengceng_detail_tmp limit 10").cache()
df.take(5)



由版本看到和base打印的不同，而且使用的是py36_dev的虚拟环境,同时可以连hive读数据。
10.开发机spark提交py3

（1）打包虚拟环境
cd /home/data/common/anaconda2/envs
tar -zcvf py36_dev.tar.gz py36_dev

          
（2）spark-submit.sh
#!/bin/bash
spark-submit --master yarn \
    --deploy-mode $1 \
    --driver-memory 8g \
    --executor-memory 8g \
    --num-executors 100 \
    --conf 'spark.dynamicAllocation.maxExecutors=800' \
    --conf 'spark.executor.memoryOverhead=50116' \
    --conf 'spark.speculation=true' \
    --queue bigquery \
    --conf 'spark.speculation.quantile=0.9' \
    --conf 'spark.speculation.multiplier=1.5' \
    --archives hdfs:/user/ethang.li/py_env/py36_dev.tar.gz#ANACONDA\
    --conf spark.yarn.appMasterEnv.PYSPARK_PYTHON=./ANACONDA/py36_dev/bin/python \
    --conf spark.yarn.appMasterEnv.PYSPARK_DRIVER_PYTHON=./ANACONDA/py36_dev/bin/python \
    --conf spark.yarn.appMasterEnv.LTP_MODEL_DIR=./ANACONDA/py36_dev/lib/python3.6/site-packages \
    $2

其中archives后面换成开发机的绝对路径也ok，比如/home/data/common/anaconda2/envs/py36_dev.tar.gz#ANACONDA。
  
（3）提交任务
提交任务时需要在py2的环境进行提交，并且deploy-mode是cluster。


提交的环境并没有jieba，而打包的环境是有jieba包的。

          
bash /home/data/ethang.li/spark-submit.sh cluster /home/data/common/spark_py3_test.py




          
（4）yarnlogs.sh
#!/bin/bash
yarn logs -applicationId $1 > /home/data/ethang.li/logs/$1

          
（5）拉取日志
bash /home/data/ethang.li/yarnlogs.sh application_1601265411830_1801688
vi /home/data/ethang.li/logs/application_1601265411830_1801688
```


          

