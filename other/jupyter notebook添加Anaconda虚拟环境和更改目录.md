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


### Reference
https://www.cnblogs.com/yinzm/p/7881328.html
https://zhuanlan.zhihu.com/p/31382311
