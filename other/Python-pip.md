### Python和pip常用命令  
python -V   查看python版本   
pip freeze > requirements.txt   命令迁移模块  
- echo -e "\033[?25l"  隐藏光标  
- echo -e "\033[?25h" 显示光标

- wget https://bootstrap.pypa.io/get-pip.py         非root用户安装pip3 
- python3 get-pip.py --user                                
### Anaconda
1. 下载Anaconda 国内镜像源  wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/包名
2. 安装Anaconda            bash 包名
3. Anaconda装完之后,开始创建环境安装tensorflow   conda create -n your_env_name python=X.X

### Reference
https://www.dazhuanlan.com/2019/08/23/5d5ec478a9cbb/
