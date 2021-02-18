### jupyter notebook添加Anaconda虚拟环境

conda create -n py3 python=3 # 创建一个python3的环境，名为py3  
source activate py3 # 激活py3环境  
conda install ipykernel # 安装ipykernel模块  
python -m ipykernel install --user --name py3 --display-name "py3" # 进行配置  
jupyter notebook # 启动jupyter notebook，然后在"新建"中就会有py3这个kernel了  

### jupyter notebook更改文件打开目录
1. 打开git bash, 输入jupyter notebook --generate-config  
2. 根据上面显示的路径，打开.jupyter 下面的配置文件jupyter_notebook_config.py修改如下  
##The directory to use for notebooks and kernels.  （这是个例子）  
c.NotebookApp.notebook_dir = 'C:\study\jupyter-notebook'  

### Conda环境移植（克隆）的方法
在本地的conda里已经有一个AAA的环境，我想创建一个新环境跟它一模一样的叫BBB，那么这样一句就搞定了：  
conda create -n BBB --clone AAA  

### jupyter中添加conda虚拟环境
conda添加
原谅我一直都没怎么用jupyter，只不过最近比赛的时候发现大佬们都在用这个东西，然后用完就无法自拔，好用的不要不要的…
但是一般都使用anconda，当出现多个环境的时候我开始从网上各种搜，然后各种垃圾教程。。。知道让我遇见了你。。（好gay…），本博最后有相关网站参考
整理如下：
前奏：自行安装anaconda，并创建虚拟环境

首先安装ipykernel
在terminal下执行命令行：conda install ipykernel
在虚拟环境下创建kernel文件
在terminal下执行命令行：conda install -n 环境名称 ipykernel
比如我的虚拟环境叫python27（后面举例都默认这个虚拟环境），那么我的就是：conda install -n python27 ipykernel
激活conda环境
在terminal下执行命令行：
windows版本:activate 环境名称 我的命令是：activate python27
linux版本:source activate 环境名称我的命令是：activate python27
将环境写入notebook的kernel中
python -m ipykernel install --user --name 环境名称 --display-name "在jupyter中显示的环境名称"
这里引号里面的名称自己可以随便起，用于在jupyter里面做标识，这里我仍然在jupyter里面叫python27，所以我的命令是：python -m ipykernel install --user --name python27 --display-name "python27"
打开notebook服务器
在terminal下执行命令行jupyter notebook
上面的相关步骤就可以完成jupyter的相关配置，但是如果经常需要用jupyter notebook，那么最好在创建虚拟环境的时候便安装好ipykernel
命令：conda create -n 环境名称 python=3.5 ipykernel

另外删除kernel环境：
jupyter kernelspec remove 环境名称

### Reference
https://www.cnblogs.com/yinzm/p/7881328.html
https://zhuanlan.zhihu.com/p/31382311
https://blog.csdn.net/u014665013/article/details/81084604
