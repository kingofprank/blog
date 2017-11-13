---
title : Caffe安装
date : 2017-11-12 15:21:00
author : mxlin
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


这里放几个教程链接吧，在我安装的过程中给了我很多帮助。
(无root权限安装caffe)[https://github.com/yosinski/caffe/blob/jason_public/doc/linux-no-root-install-log.md]
(Python3.5 Anaconda3 Caffe深度学习框架搭建)[http://yingshu.ink/2017/01/12/Python3-5-Anaconda3-Caffe%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E6%A1%86%E6%9E%B6%E6%90%AD%E5%BB%BA/]
(caffe官方安装教程)[http://caffe.berkeleyvision.org/installation.html]



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

CUDA® 是NVIDIA 创造的一个并行计算平台和编程模型。它利用图形处理器(GPU) 能力，实现计算性能的显著提高。由于系统已经将cuda配置好了，所以这里我只需要将cuda路径加入$PATH和$LD_LIBRARY_PATH，分别是`/usr/local/cuda-8.0/bin`和`/usr/local/cuda-8.0/lib64`。

(NVIDIA驱动安装命令)[https://github.com/floydhub/dl-setup#nvidia-drivers]

(cuda安装命令)[https://github.com/floydhub/dl-setup#cuda]


# cuDNN

CuDNN是专门针对Deep Learning框架设计的一套GPU计算加速方案，目前支持的DL库包括Caffe，ConvNet, Torch7等。同样的系统已经安装了，可以通过`nvidia-smi`命令检查是否安装完毕。

(cuDNN安装命令)[https://github.com/floydhub/dl-setup#cudnn]


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

这里坑了我两天，由于系统的版本不对，是2.4.0的，所以用系统的protobuf来编译pycaffe就会出import问题，前面make都很顺利，但是在import的时候就会报出google.protobuf模块中的引用问题。原因是编译的时候是用2.4.0的，但是python调用的时候是用conda安装的protobuf，是3.4.0的，所以发生了冲突。要升级到3.0以上才可以。

解决办法是修改路径，由于protobuf是通过anaconda中的pip安装的。这里还有一个疑惑是libprotobuf和protobuf到底有什么区别，这个和libboost与boost应该是一样的区别，但是实际上可以用的是libprotobuf和libboost，因为它并没有安装到`~/anaconda3/lib`下，而是在`~/anaconda3/pkgs/libprotobuf-3.4.0-0/lib`中，故将其添加到环境变量中，也将库文件添加进去。之后通过`protoc --version`命令就可以看得到调用的protobuf已经变成了anaconda中安装的了。

主要的核心问题就是要让python调用pycaffe时使用的版本，与编译pycaffe时使用的版本一致。因为编译的时候是用系统的路径，import的时候使用python的路径，产生了冲突。

貌似libprotobuf和protobuf都要安装，是通过protobuf-cpp-3.4.0.zip和protobuf-python-3.4.0.zip两个版本分别编译出来的。

```
解压安装包，进入解压的目录
$ cd python
$ python setup.py build
$ python setup.py test
$ python setup.py install
```
这样protobuf python runtime就编译和安装好了。注意protobuf python runtime是作为pip的包安装的。但是你可以从conda里面看到他(这句话节选自(Python3-5-Anaconda3-Caffe深度学习框架搭建)[http://yingshu.ink/2017/01/12/Python3-5-Anaconda3-Caffe%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E6%A1%86%E6%9E%B6%E6%90%AD%E5%BB%BA/])


# hdf5

这里我已经不想写了，和boost一样，需要保持版本一致即可。


#Openblas

这个库算是给我造成问题最小的了，在caffe中有三个可以选择，一个是python自带的atlas或者Intel的MKL，以及OpenBlAS。我这里选择了手动编译，安装Openblas。

```
git clone https://codeload.github.com/xianyi/OpenBLAS/tar.gz/v0.2.20.git

cd OpenBLAS
  
make FC=gfortran -j $(($(nproc) + 1))
  
make PREFIX=~/Openblas install
```

然后添加OpenBlas到动态路径中
```
echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
```

(Openblas安装命令)[https://github.com/floydhub/dl-setup#openblas]


# Opencv
同样的，在系统中就已经装了opencv2.4.8版本的了，不过同样在anaconda中已经有opencv3.1.0了，于是可以覆盖掉系统的版本。
但是需要手动的编译opencv。

(官方网站)[http://blog.csdn.net/yhaolpz/article/details/71375762]

解压到要安装的位置，执行
```
mkdir build

cd build

make -D -DBUILD_TIFF=ON ...

make install
```

注意WITH_TBB模块在3.0以下版本要选择ON,WITH_QT如果之前没装qt的话要选择OFF，再把CMAKE_INSTALL_PREFIX改为/home/yourname/local_install , 其他模块自行选择。

这里主要是要手动的打开`-DBUILD_TIFF=ON`，不然会在make caffe的时候报错`undefined reference toTIFFRGBAImageOK@LIBTIFF_4.0'`。

其实这里也存在疑惑，先记录一下，我在anaconda中安装了opencv，python也是调用anaconda下的opencv。在编译的时候如果不手动编译的话，就会报错，但是我手动编译之后，用`pkg-config --modversion opencv`命令查看opencv版本的时候仍然是2.4.8版本的，这可能是因为我没有用sudo命令安装有关，我安装的是在用户根目录下，所以pkg工具找不到。但是因为我把anaconda添加到$PATH中，所以我觉得在caffe编译的时候还是会先找anaconda下的opencv，但是这里就不知道手动编译的opencv去哪里了。。。



# Caffe

终于万事俱备了，Caffe从很早以前就听说是一个很困难的东西，安装起来非常的不方便（所以搞上了TensorFlow）。这次一试果然很困难啊，但其实因为有很多linux下的东西自己不太熟悉。其实当依赖库都正确安装后，步骤非常的简单。

首先到(Caffe官网)[https://github.com/BVLC/caffe]下载
接着运行`cp Makefile.config.example Makefile.config`创建一个Makefile.config文件，打开后进行如下修改:

```
  1 ## Refer to http://caffe.berkeleyvision.org/installation.html
  2 # Contributions simplifying and improving our build system are welcome!
  3 
  4 # cuDNN acceleration switch (uncomment to build with cuDNN).
  5 USE_CUDNN := 1
  6 
  7 # CPU-only switch (uncomment to build without GPU support).
  8 # CPU_ONLY := 1
  9 
 10 # uncomment to disable IO dependencies and corresponding data layers
 11 # USE_OPENCV := 0
 12 # USE_LEVELDB := 0
 13 # USE_LMDB := 0
 14 
 15 # uncomment to allow MDB_NOLOCK when reading LMDB files (only if necessary)
 16 #       You should not set this flag if you will be reading LMDBs with any
 17 #       possibility of simultaneous read and write
 18 # ALLOW_LMDB_NOLOCK := 1
 19 
 20 # Uncomment if you're using OpenCV 3
 21 OPENCV_VERSION := 3
 22 
 23 # To customize your choice of compiler, uncomment and set the following.
 24 # N.B. the default for Linux is g++ and the default for OSX is clang++
 25 # CUSTOM_CXX := g++
 26 
 27 # CUDA directory contains bin/ and lib/ directories that we need.
 28 CUDA_DIR := /usr/local/cuda
 29 # On Ubuntu 14.04, if cuda tools are installed via
 30 # "sudo apt-get install nvidia-cuda-toolkit" then use this instead:
 31 # CUDA_DIR := /usr
 32 
 33 # CUDA architecture setting: going with all of them.
 34 # For CUDA < 6.0, comment the *_50 through *_61 lines for compatibility.
 35 # For CUDA < 8.0, comment the *_60 and *_61 lines for compatibility.
 36 CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
 37                 -gencode arch=compute_20,code=sm_21 \
 38                 -gencode arch=compute_30,code=sm_30 \
 39                 -gencode arch=compute_35,code=sm_35 \
 40                 -gencode arch=compute_50,code=sm_50 \
 41                 -gencode arch=compute_52,code=sm_52 \
 42                 -gencode arch=compute_60,code=sm_60 \
 43                 -gencode arch=compute_61,code=sm_61 \
 44                 -gencode arch=compute_61,code=compute_61
 45 
 46 # BLAS choice:
 47 # atlas for ATLAS (default)
 48 # mkl for MKL
 49 # open for OpenBlas
 50 BLAS := open
 51 # Custom (MKL/ATLAS/OpenBLAS) include and lib directories.
 54 BLAS_INCLUDE := /home/mxlin/OpenBLAS/include
 55 BLAS_LIB := /home/mxlin/OpenBLAS/lib
 56 
 57 # Homebrew puts openblas in a directory that is not on the standard search path
 58 # BLAS_INCLUDE := $(shell brew --prefix openblas)/include
 59 # BLAS_LIB := $(shell brew --prefix openblas)/lib
 60 
 61 # This is required only if you will compile the matlab interface.
 62 # MATLAB directory should contain the mex binary in /bin.
 63 # MATLAB_DIR := /usr/local
 64 # MATLAB_DIR := /Applications/MATLAB_R2012b.app
 65 
 66 # NOTE: this is required only if you will compile the python interface.
 67 # We need to be able to find Python.h and numpy/arrayobject.h.
 68 # PYTHON_INCLUDE := /usr/include/python2.7 \
 69                 /usr/lib/python2.7/dist-packages/numpy/core/include
 70 # Anaconda Python distribution is quite popular. Include path:
 71 # Verify anaconda location, sometimes it's in root.
 72 ANACONDA_HOME := $(HOME)anaconda3
 73 PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
 74                   $(ANACONDA_HOME)/include/python3.6m \
 75                   $(ANACONDA_HOME)/lib/python3.6/site-packages/numpy/core/include
 76 
 77 # Uncomment to use Python 3 (default is Python 2)
 78 PYTHON_LIBRARIES := boost_python3 python3.6m
 79 # PYTHON_INCLUDE := /usr/include/python3.5m \
 80 #                 /usr/lib/python3.5/dist-packages/numpy/core/include
 81 
 82 # We need to be able to find libpythonX.X.so or .dylib.
 83 # PYTHON_LIB := /usr/lib
 84 PYTHON_LIB := $(ANACONDA_HOME)/lib
 85 
 86 # Homebrew installs numpy in a non standard path (keg only)
 87 # PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include
 88 # PYTHON_LIB += $(shell brew --prefix numpy)/lib
 89 
 90 # Uncomment to support layers written in Python (will link against Python libs)
 91 WITH_PYTHON_LAYER := 1
 92 
 93 # Whatever else you find you need goes here.
 94 INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
 95 LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/
 96 
 97 # If Homebrew is installed at a non standard location (for example your home directory) and you use it for general d    ependencies
 98 # INCLUDE_DIRS += $(shell brew --prefix)/include
 99 # LIBRARY_DIRS += $(shell brew --prefix)/lib
100 
101 # NCCL acceleration switch (uncomment to build with NCCL)
102 # https://github.com/NVIDIA/nccl (last tested version: v1.2.3-1+cuda8.0)
103 # USE_NCCL := 1
104 
105 # Uncomment to use `pkg-config` to specify OpenCV library paths.
106 # (Usually not necessary -- OpenCV libraries are normally installed in one of the above $LIBRARY_DIRS.)
107 # USE_PKG_CONFIG := 1
108 
109 # N.B. both build and distribute dirs are cleared on `make clean`
110 BUILD_DIR := build
111 DISTRIBUTE_DIR := distribute
112 
113 # Uncomment for debugging. Does not work on OSX due to https://github.com/BVLC/caffe/issues/171
114 # DEBUG := 1
115 
116 # The ID of the GPU that 'make runtest' will use to run unit tests.
117 TEST_GPUID := 0
118 
119 # enable pretty build (comment to see full commands)
120 Q ?= @

```


1、使用cudnn时，将第5行USE_CUDNN注释去掉。
2、使用opencv3的时候，将第21行OPENCV_VERSION注释去掉。
3、将28行，CUDA_DIR改成自己的CUDA目录，不过应该都是默认的
4、将50行，BLAS选择自己安装的库，我这里选择open，然后在接下来的54和55行改成自己的安装位置。
5、最重要的，第72行，这里改的是python的头文件链接位置，若使用anaconda的话，改成自己的ANACONDA_HOME位置，注意要看看有没有这个文件夹，同样的改PYTHON_INCLUDE。
6、之后的78行，这里改的是python的链接库位置，如果使用python3，则要添加动态链接库的位置，这里应当要选择anaconda3目录下的，当前环境下的python库位置！！！我第一次错误的去找了系统的位置，这个位置在`~/anaconda3/lib`下，存在一个libboost_python3.so和libpython3.6m的库文件，按照自己的安装文件改就行。
6、在84行，同样是改库文件位置，选择HOME下的lib即可。
7、在第91行，若要打算使用pycaffe，则要取消注释。
8、在94和95行，则是要把系统的一切链接库位置都写上去，因为前面有python的位置，所以会屏蔽系统的库文件位置，但是有一些依赖库，还是需要用系统的吧(我猜的)。

以上便是所有的修改环节，讲起来容易，但其实坑了很久，接下来便开始正式的编译啦！。

```
make clean //如果之前有过编译的话，要清楚原来的历史编译版本
make all -j16 //j16是使用16个线程，看个人机器，我这服务器是16个核。
make runtest -j16 //可以做也可以不做，是用来做测试的，看看会不会出现什么问题。
make runtest -j16 //跑测试
make pycaffe -j16 //编译pycaffe
```

最后将caffe路径添加到环境变量中，才可以import进来
```
#Caffe Root
export CAFFE_ROOT="/home/mxlin/caffe"
export PYTHONPATH="/home/mxlin/caffe/python:$PYTHONPATH"
```
记得`source ~/.bashrc`

在编译的过程中，可能会有各种错误提示，按照提示去google即可。

最后，打开python，输入import caffe，若没有报错则大功告成！。

# 遇到的问题

1、在make caffe的时候报`include "caffe/proto/caffe.pb.h"`无法生成的问题。
也不知道什么原因，我猜还是因为protobuf版本不一致的原因吧，手动编译出该文件即可。
```
protoc src/caffe/proto/caffe.proto --cpp_out=.

mkdir include/caffe/proto

mv src/caffe/proto/caffe.pb.h include/caffe/proto`
```

2、import caffe的时候报`Can't create weekday with n == 0`
这个是protobuf版本的问题，当我使用了低于3.0版本的protobuf的时候，就会报这个问题，当使用大于3.0版本的时候，会提示在google.protobuf文件中某处引用模块出现问题，解决办法就是让编译版本和运行版本保持一致。

3、import caffe的时候报undefined symbol: _ZN5boost6python6detail11init_moduleER11PyModuleDefPFvvE

提示boost版本不匹配，这里是因为在Makefile.config中没有正确的配置PYTHON_LIBRARIES，按照上文配置即可。

4、libhdf5_hl.so.10: cannot open shared object file: No such file or directory
这个就是hdf5版本不对的问题了，同样是要用自己环境下的hdf5，需要更改路径，掩盖掉系统的。


# 后记
这次安装经历，让我学到了许多东西，起码更熟悉了点linux系统，其实caffe安装并不是很难，控制好版本的问题,虽然我觉得可能在后续的使用中还会有问题，(因为我写完这篇之后并不觉得我完全正确的安装了)，但基本已经熟悉了套路了，在这里mark一下，以便以后回顾。


