1.查看cuda版本（在ubuntu下）：

cat /usr/local/cuda/version.txt
或
nvcc -V

2.查看cudnn版本（在ubuntu下）：  
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

3.查看tensorflow-gpu的版本：  
pip list
