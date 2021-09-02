###  linux bashrc设置

```
# .bashrc

# Source global definitions
export SPARK_HOME=/usr/hdp/3.1.4.0-315/spark2

export PYSPARK_PYTHON=python2

export PYSPARK_DRIVER_PYTHON=python2

export PATH="/home/data/common/anaconda2/bin:$PATH"

if [ -f /etc/bashrc ]; then
    . /etc/bashrc
fi

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/data/common/anaconda2/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/data/common/anaconda2/etc/profile.d/conda.sh" ]; then
        . "/home/data/common/anaconda2/etc/profile.d/conda.sh"
    else
        export PATH="/home/data/common/anaconda2/bin:$PATH"
        #export PATH=$PATH
    fi  
fi
unset __conda_setup
# <<< conda initialize <<<

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
```
