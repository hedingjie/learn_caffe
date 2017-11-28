## Caffe安装教程（CPU-only）

### 前言
很多同学在学习caffe的过程中会发现，整个过程中遇到的最大的困难不在于怎样使用这样一个框架以及它的理论基础，而是安装caffe。对！你们有听错，安装caffe绝对是大部分人自接触计算机以来所遇到的最难安装的软件，没有之一。所以在接下来的教程中，要是你花上大半天或者一天的时间来安装软件，那恭喜你，你是幸运的，因为我花了4天 :-(
### 安装环境
* Ubuntu 16.04 LST
* 内存：2GB
* 处理核心数：2

（其实这是一台安装在mac上ubuntu虚拟机）

### 安装必要的软件包
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
### 安装OpenCV 3.1
caffe作为机器学习的框架，在图像识别分类领域有着广泛的应用。当然，这离不开机器视领域久负盛名的库——OpenCV的支持。接下来就是要通过源码编译安装OpenCV 3.1 （[点击下载](https://codeload.github.com/opencv/opencv/zip/3.1.0)）。

下载下来zip包后，先进行解压缩
```unzip 3.1.0```
之后，进入解压后的目录，如图所示：




