### 克隆模块中包含子模块
1) git clone --recursive https://github.com/example/example.git
2) git submodule init和git submodule update

### 克隆不同分支的模块
git clone -b <指定分支名> <远程仓库地址>

### 生成新的SSH key
首先在终端输入 ssh-keygen -t rsa -C "your_email@example.com"

### git中submodule子模块的添加、使用和删除

项目中经常使用别人维护的模块，在git中使用子模块的功能能够大大提高开发效率。

使用子模块后，不必负责子模块的维护，只需要在必要的时候同步更新子模块即可。

本文主要讲解子模块相关的基础命令，详细使用请参考man page。

子模块的添加
添加子模块非常简单，命令如下：

git submodule add <url> <path>

其中，url为子模块的路径，path为该子模块存储的目录路径。

执行成功后，git status会看到项目中修改了.gitmodules，并增加了一个新文件（为刚刚添加的路径）

git diff --cached查看修改内容可以看到增加了子模块，并且新文件下为子模块的提交hash摘要

git commit提交即完成子模块的添加

子模块的使用
克隆项目后，默认子模块目录下无任何内容。需要在项目根目录执行如下命令完成子模块的下载：

git submodule init
git submodule update
或：
git submodule update --init --recursive
执行后，子模块目录下就有了源码，再执行相应的makefile即可。

子模块的更新
子模块的维护者提交了更新后，使用子模块的项目必须手动更新才能包含最新的提交。

在项目中，进入到子模块目录下，执行 git pull更新，查看git log查看相应提交。

完成后返回到项目目录，可以看到子模块有待提交的更新，使用git add，提交即可。

删除子模块
有时子模块的项目维护地址发生了变化，或者需要替换子模块，就需要删除原有的子模块。

删除子模块较复杂，步骤如下：

rm -rf 子模块目录 删除子模块目录及源码
vi .gitmodules 删除项目目录下.gitmodules文件中子模块相关条目
vi .git/config 删除配置项中子模块相关条目
rm .git/module/* 删除模块下的子模块目录，每个子模块对应一个目录，注意只删除对应的子模块目录即可
执行完成后，再执行添加子模块命令即可，如果仍然报错，执行如下：

git rm --cached 子模块名称

完成删除后，提交到仓库即可。


### git submodule添加子模块(子仓库)
在父仓库中添加子仓库

创建父仓库(parent)
clone地址:: https://gitee.com/xxx/parent.git

创建子仓库(child1)
clone地址:: https://gitee.com/xxx/child1.git

创建子仓库(child2)
clone地址:: https://gitee.com/xxx/child2.git

注: 创建仓库的时候勾选README.md,保证不是空仓库

父仓库添加子仓库依赖
第一步: 克隆父仓库
$ git clone https://gitee.com/xxx/parent.git
第二步: 添加子仓库
$ git submodule add https://gitee.com/xxx/child1.git
$ git submodule add https://gitee.com/xxx/child2.git
第三步: push到父仓库
$ git add .
$ git commit -m ‘add submodule’
$ git push

注: 当第二步(添加子仓库)操作完成之后,项目中将生成…gitmodules文件
文件内容如下:
[submodule “child1”]
path = child1
url = https://gitee.com/xxx/child1.git
[submodule “child2”]
path = child2
url = https://gitee.com/xxx/child2.git

递归克隆父仓库
$ git clone --recursive https://gitee.com/xxx/parent.git

更新子仓库
6.1 更新已存在的仓库代码
git submodule foreach git pull origin master
6.2 更新新增的子仓库代码
git submodule update

### Reference
https://blog.csdn.net/guotianqing/article/details/82391665
https://blog.csdn.net/qq_37751454/article/details/103439409
