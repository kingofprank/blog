---
title : Caffe安装
date : 2017-11-12 15:21:00
---

这几天学着如何配置TensorFlow和Caffe环境，学到了许多有用的东西。因为在服务器上安装，没有root权限，整整折腾了这个东西三四天，才总算把Caffe装好。过程极其坎坷。下面简单介绍一下在这次配置中所用到的工具和内容，以及遇到的一些问题。

<!--more-->

首先先介绍一下我所用的环境
```
Linux Mint 17.3 Rosa
Anaconda3
Python 3.6.3
gcc 4.8.4
g++ 4.9.4
NVIDIA UNIX x86_64 Kernel Module  375.66 // Nvidia驱动版本
Cuda8.0
cuDNN6.0.21
OpenBLAS-0.2.20
Opencv3.1.0
protobuf3.4.0
boost
```


# Anaconda3

从师兄那知道了Anaconda这个管理工具，可以很方便的配置不同环境中的python，下面简单的介绍一下。Anaconda是一款包管理和环境管理工具，能轻松的解决python多环境下管理包的问题。

Anaconda分2和3，分别是默认不同版本的python。我这里使用的是Anaconda3，自带python3.4.5。

[官网地址](https://www.anaconda.com/download/)

安装过程默认就行，不使用root权限（由于是在服务器上，也没有root权限），Anaconda3会在`~/anaconda3`生成文件，安装过程中会有一个选项，是否把程序bin目录加入`$PATH`，输入yes，在linux中会写入`~/.bashrc`，安装之后记得查看一下。之后记得用`source ~/.bashrc`命令更新一下环境变量。


下面给出几个常用的命令
```
conda --version // 查看版本

# 环境管理

conda create --name tensorflow python=3.4 // 创建一个名为tensorFlow的环境，同时指定python版本为3.4.x，conda会自动安装3.4.x中最新的版本。

source activate tensorflow // 激活tensorflow环境

source deactivate tensorflow // 返回默认环境，Linux和Mac环境下返回命令
deactivate tensorflow // Windows下返回命令

conda remove --name tensorflow -all // 删除一个已有的环境

conda info -e // 查看安装的环境


# 包管理

conda install PackageName // 在当前环境下安装包，conda会自动加载所需要的依赖库，并且自动更新，相当的方便。可以令PackageName=x.x.x 指定版本更新

conda remove/uninstall PackageName //指定删除当前环境下的某个包，同时也会删除依赖库

conda list // 查看该环境下，安装的包，是从site-packages文件夹中搜索已经安装的包

conda update PackageName //更新包，也可以更新conda

以上命令都可以添加 -n EnvName 来指定在某个环境中进行


# 修改镜像地址

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ // 添加清华的镜像，加快下载速度，TUNA的help中镜像地址加有引号，需要去掉

conda config --set show_channel_urls yes // 设置搜索时显示的通道地址

在对conda执行上述命令后，会在~/.condarc或者C:\Users\USER_NAME\.condarc添加进路劲信息，如下所示

 1 channels:
 2   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
 3   - defaults
 4 ssl_verify: true

```

不同的环境下安装的都会放在`~/anaconda3/envs`下，会有一个带有该环境名字的文件夹。到此为止，基本环境已经搭建好了，下面需要介绍几个重要的依赖库。

# cuda8.0

。。。

# cuDNN

。。。

# Boost
在我的环境下，安装了2个版本的boost。
第一个是系统默认的1.54.0。(头文件在`/usr/include/boost`,函数库文件在`/usr/lib/x86_64-linux-gnu`)
第二个是anaconda默认环境下的1.61.0。（头文件在`~/anaconda3/include/boost`,库文件在`~/anaconda3/lib`）

我这里使用的是1.61.0的boost版本，这里因为在环境变量中设置了$PATH,所以anaconda的boost覆盖了系统的boost。这里要着重说明的一点是，因为boost在$PATH路径覆盖的范围内，所以覆盖了系统的boost。但是之后的protobuf，由于头文件和库文件都没有安装到`~/anaconda3/include`和`~/anaconda3/lib`下所以无法被调用，这导致了我安装上巨大的问题，坑了好多天！！


`~/anaconda3/pkgs`文件夹中放的是conda下载的压缩包和解压的文件夹。在这个文件夹下我有`boost-1.61.0-py36_0`，但又有一个叫`libboost-1.65.1-h4055789_3`的，貌似是我通过conda安装的，但是也不知道libboost文件有什么用处，先不管了。

通过命令
`conda install boost`
安装在该环境下。

# Protobuf

大坑，待更。。

# hdf5

待更。。

# Opencv
同样的，在系统中就已经装了opencv2.4.8版本的了，不过同样在anaconda中已经有opencv3.1.0了，于是可以覆盖掉系统的版本。
但是需要手动的编译opencv。

(官方网站)[http://blog.csdn.net/yhaolpz/article/details/71375762]

解压到要安装的位置，执行
```
mkdir build

cd build

make -D -DBUILD_TIFF=ON

make install
```

这里主要是要手动的打开`-DBUILD_TIFF=ON`，不然会在make caffe的时候报错`undefined reference toTIFFRGBAImageOK@LIBTIFF_4.0'`。

其实这里也存在疑惑，先记录一下，我在anaconda中安装了opencv，python也是调用anaconda下的opencv。在编译的时候如果不手动编译的话，就会报错，但是我手动编译之后，用`pkg-config --modversion opencv`命令查看opencv版本的时候仍然是2.4.8版本的，这可能是因为我没有用sudo命令安装有关，我安装的是在用户根目录下，所以pkg工具找不到。但是因为我把anaconda添加到$PATH中，所以我觉得在caffe编译的时候还是会先找anaconda下的opencv，但是这里就不知道手动编译的opencv去哪里了。。。



# Caffe
Caffe从很早以前就听说是一个很困难的东西，安装起来非常的不方便（所以搞上了TensorFlow）。

首先是要安装cuda，这里因为在服务器上已经安装好了cuda-8.0和cudnn，最困难的部分已经搞定了。

。。。

