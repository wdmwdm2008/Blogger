1. ImportError: No module named tensorflow.compat.v1 忽略已经安装的某个包版本 忽略已安装版本  
pip install --ignore-installed --upgrade --ignore-installed tensorflow
2. ubuntu: 查看cuda版本  
nvcc -V
3. 查看gpu和memory详细情况  
nvidia-smi
### Python和pip常用命令  
python -V   查看python版本   
pip freeze > requirements.txt   命令迁移模块  
- echo -e "\033[?25l"  隐藏光标  
- echo -e "\033[?25h" 显示光标

- wget https://bootstrap.pypa.io/get-pip.py         非root用户安装pip3 
- python3 get-pip.py --user        
