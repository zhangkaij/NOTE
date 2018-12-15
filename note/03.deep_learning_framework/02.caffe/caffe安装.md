#Caffe
&emsp;&emsp;目标检测领域多使用caffe，由于复现论文里面的实验成本太高，故由pytorch转到caffe。使用anaconda管理python包。

## 环境
* Ubuntu 14.04.5 LTS
* conda 4.3.27
* python 2.7
* opencv 2.4.11
* cuda8.0

&emsp;&emsp;不建议使用python3，问题比较多。使用opencv3，一直有问题，最后果断换为opencv2.4.11。

## 安装
&emsp;&emsp;按照官网的要求安装依赖包。
```language
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libhd5-serial-dev libopencv-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```

&emsp;&emsp;使用anaconda安装python包，文件./python/requirements.txt中包含了需要安装的软件包，anaconda已经默认安装了绝大部分软件包。可以使用命令conda list | grep ***(包名)确认软件包是否已经安装。对于我的环境来说只需要安装leveldb。由于已经安装了libprotobuf-dev和protobuf-compiler，为了避免编译时版本冲突，不要安装protobuf软件包。
```language
conda install leveldb
conda remove protobuf
```
&emsp;&emsp;安装opencv2.4.11
```language
conda install -c menpo opencv #安装的opencv为2.4版本
#conda install opencv 安装的opencv为3.1版本
```

## 配置Caffe
```
1. 取消注释
USE_CUDNN := 1
2. 添加注释
# PYTHON_INCLUDE := /usr/include/python2.7 \
#		/usr/lib/python2.7/dist-packages/numpy/core/include
3. 取消注释
#如果没有使用虚拟环境，需要做如下修改ANACONDA_HOME:=$(HOME)/<username>/software/anaconda3
ANACONDA_HOME := $(HOME)/<username>/software/anaconda3/envs/py27
PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
		 $(ANACONDA_HOME)/include/python2.7 \
		 $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include
4. PYTHON_LIB应该为
# PYTHON_LIB := /usr/lib
PYTHON_LIB := $(ANACONDA_HOME)/lib
5. 使用pycaffe的需要取消如下注释
WITH_PYTHON_LAYER := 1
6. 使用anaconda安装python软件包，需要取消此行的注释
PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include
7. 头文件包含为
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/x86_64-linux-gnu
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib
```

##编译Caffe
```language
make all -j8 #使用8个进程编译
make pycaffe
make distribute
# 测试
make test
make runtest
```
## 修改环境变量
在文件~/.bashrc中添加如下语句
```language
export CAFFE_ROOT=/home/zhangkj/software/caffe
export PYTHONPATH=/home/zhangkj/software/caffe/python:$PYTHONPATH
export PATHONPATH=/home/zhangkj/software/caffe/distribute/python:$PATHONPATH
```
更新~/.bashrc文件
```language
source ~/.bashrc
```
## 测试caffe
```language
cd ~/caffe
./data/mnist/get_mnist.sh
./examples/mnist/create_mnist.sh
./examples/mnist/create_mnist.sh
```
## 遇到的问题

问题1、安装python软件包protobuf，导致生成的protobuf版本较新，libprotobuf-dev为2.5版本，导致make all失败。
方法：

```language
1. 确认libprotobuf-dev版本
dpkg -s libprotobuf-dev
2. 确认protobuf版本
conda list | grep protobuf
3. 删除protobuf
conda remove protobuf
```

问题2、fatal error: caffe/proto/caffe.pb.h: No such file or directory
方法：
需要使用==protoc==手动生成==caffe.pb.h==:
```language
# In the directory you installed caffe to
protoc src/caffe/proto/caffe.proto --cpp_out=.
mkdir include/caffe/proto
mv src/caffe/proto/caffe.pb.h include/caffe/proto
```

问题3、**/wrap_python.hpp:50:23:fatal error: pyconfig.h:No such file or directory
方法：
$ export CPLUS_INCLUDE_PATH=~/software/anaconda3/envs/py27/include/python2.7

问题4、make pycaffe时numpy/arrayobject.h: No such file or directory
方法：Makeflie.config中把该行的注释
>PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include

取消掉。
问题5、error while loading shared libraries: libhdf5_hl.so.10: cannot open shared object file: No such file or directory
方法：
该问题多出现在使用anaconda安装Python包。可以在～/.bashrc中添加
“export LD_LIBRARY_PATH=/home/zhangkj/anaconda/lib:$LD_LIBRARY_PATH"













