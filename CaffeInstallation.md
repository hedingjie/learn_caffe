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

## 安装Caffe
终于进入主题，安装Caffe了！但是正如之前所说，这似乎并不像我们想象的那么简单。首先，我们需要在自己想要安装的路径下[下载](https://codeload.github.com/BVLC/caffe/zip/master)caffe的源码下来，同样进行解压缩之后进入解压目录，可以看到有一个Make



