# Caffe安装教程（CPU-only）

## 前言
很多同学在学习caffe的过程中会发现，整个过程中遇到的最大的困难不在于怎样使用这样一个框架以及它的理论基础，而是安装caffe。对！你们有听错，安装caffe绝对是大部分人自接触计算机以来所遇到的最难安装的软件，没有之一。所以在接下来的教程中，要是你花上大半天或者一天的时间来安装软件，那恭喜你，你是幸运的，因为我花了4天 :-(
## 安装环境
* Ubuntu 16.04 LST
* 内存：2GB
* 处理核心数：2

（其实这是一台安装在mac上ubuntu虚拟机）

## 安装必要的软件包
首先，我们需要安装相应的依赖包，不过在这之前，先得进行更新：
```
	sudo apt-get update
```

在这之后依次执行以下命令：
```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
```
```
sudo apt-get install --no-install-recommends libboost-all-dev
```
```
sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
```
```
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```
```
sudo apt-get install git cmake build-essential
```
```
sudo apt-get install git cmake build-essential
```
## 安装OpenCV 3.1
caffe作为机器学习的框架，在图像识别分类领域有着广泛的应用。当然，这离不开机器视领域久负盛名的库——OpenCV的支持。接下来就是要通过源码编译安装OpenCV 3.1 （[点击下载](https://codeload.github.com/opencv/opencv/zip/3.1.0)）。

下载下来zip包后，先进行解压缩
```unzip 3.1.0```
之后，进入解压后的目录，如图所示：
![](https://github.com/hedingjie/learn_caffe/blob/master/res/QQ20171128-170411%402x.png)
在该目录下执行以下命令：

```
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
make -j2
```
在执行上面的命令时，由于某些原因，有一个叫做ippicv的包下载不下来，这里你可以手动进行[下载](https://raw.githubusercontent.com/Itseez/opencv_3rdparty/81a676001ca8075ada498583e4166079e5744668/ippicv/ippicv_linux_20151201.tgz)，然后将它直接放到当前目录的/3rdparty/ippicv/目录下面，然后重新执行cmake和make命令。最后执行安装：

```
sudo make install #安装
```

安装完成后可以通过```pkg-config --modversion opencv```检查OpenCV是否安装成功。

在这之后，我们指定一些caffe将来会用到的库文件的位置，其中包括一个libopencv_core.so.3.1的文件，我们首先要找到这个文件的位置，通过以下命令：
```sudo find / -name "libopencv_core.so.3.1*"```。通常会得到如图结果：

![](https://github.com/hedingjie/learn_caffe/blob/master/res/QQ20171128-195031%402x.png)

即位置为/usr/local/lib。然后创建一个文件/etc/ld.so.conf.d/opencv.conf，并添加以下内容```/usr/local/lib/```，并执行命令：```sudo ldconfig -v```。

## 安装Caffe
终于进入主题，安装Caffe了！但是正如之前所说，这似乎并不像我们想象的那么简单。首先，我们需要在自己想要安装的路径下[下载](https://codeload.github.com/BVLC/caffe/zip/master)caffe的源码下来，同样进行解压缩之后进入解压目录，如图所示：

![](https://github.com/hedingjie/learn_caffe/blob/master/res/QQ20171128-192255%402x.png)
在这个目录下面有一个python文件夹，进入python文件夹，里面有一个requirements.txt文件，这里面都是所需要的Python包，我们采用pip进行安装。不过在这之前，我们得先安装pip：

```sudo apt-get install python-pip```

在这之后，在终端执行如下语句：

```
for req in $(cat requirements.txt); do pip install $req; done 
```

在上一级目录中，可以看到有一个Makefile.config.example文件，这是配置文件的模板，我们拷贝一份并命名为Makefile.config。然后做如下更改：

* 将```# CPU_ONLY = 1```前的#号删掉
* 将```# OPENCV_VERSION := 3```前的#号删掉
* 将```PYTHON_INCLUDE := /usr/include/python2.7 \		/usr/lib/python2.7/dist-packages/numpy/core/include```改为```PYTHON_INCLUDE := /usr/include/python2.7 \		/usr/local/lib/python2.7/dist-packages/numpy/core/include```
* 将```INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/includeLIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib ```改为```INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serialLIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial /usr/local/share/OpenCV/3rdparty/lib/```
经过如上的操作，我们已经基本完成了所需要的配置。接下来就是进行编译安装了：

```
make pycaffe -j2
make all -j2
make test -j2
make runtest -j2
```
## 测试
如果上面的步骤没有报错，那么接下来配置caffe的python接口路径：

```
export PYTHONPATH=/path/to/caffe/python:$PYTHONPATH  
```

在python命令行中，输入```import caffe```来测试配置是否成功。
如果报错，尝试如下命令：

```
#A.把环境变量路径放到 ~/.bashrc文件中  
sudo echo export PYTHONPATH="~/caffe/python" >> ~/.bashrc  
#B.使环境变量生效  
source ~/.bashrc 
```

## 参考资料
* [Ubuntu16.04安装Caffe(CPU Only)](http://blog.csdn.net/muzilinxi90/article/details/53673184)
* [Ubuntu 16.04 or 15.10 Installation Guide](https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide)
* [ Ubuntu16.04 Caffe 安装步骤记录（超详尽）](http://blog.csdn.net/yhaolpz/article/details/71375762)
* [OpenCV runtime error: "libopencv_core.so.3.2: cannot open shared object file: No such file or directory" in Fedora 24 ](https://github.com/GaoHongchen/DIPDemoQt5/issues/1)


		


