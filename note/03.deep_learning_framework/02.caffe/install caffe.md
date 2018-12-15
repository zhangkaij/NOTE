

# Caffe安装
&emsp;&emsp;caffe可以同pycaffe和mtcaffe，本文安装了pycaffe，并未安装matcaffe。
## 安装环境
* ubuntu 14.04
* cuda8.0
* conda4.3.1
* python3.6

## 基本配置
1. 安装anaconda。使用anaconda管理Python包非常方便，官网下载[anaconda](https://www.anaconda.com/download/)。下载完成后，使用如下命令安装Anaconda:
> bash Anaconda3-4.4.0-Linux-x86_64.sh

2. 安装cuDNN。
> conda install cuda80 -c soumith

3. 安装依赖项。在ubuntu14.04中caffe所需要的所有事情都打包好了，可以执行如下命令安装：
> sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

4. 安装Python依赖包。如果将来准备使用pycaffe，必须安装caffe/python/requires.txt中所有依赖包。由于anaconda已经安装了绝大多数依赖包。可以通过conda --version来查看已经安装的软件包。然后使用conda install 安装未安装的软件包。我的环境只有leveldb和protobuf没有安装。使用如下命令安装：
> conda install leveldb protobuf

5. 修改配置文件Makefile.config

```
1. 取消注释 USE_CUDNN :=1
2. 注释掉： PYTHON_INCLUDE := /usr/include/python2.7 \
					/usr/lib/python2.7/dist-packages/numpy/core/include
3. 取消注释： 
ANACODA_HOME := $(HOME)/anaconda3
PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
	$(ANACONDA_HOME)/include/python3.6m \
    $(ANACONDA_HOME)/lib/python3.6m/site-packages/numpy/core/include
    # 由于默认为Python2.7，我使用的为Python3.6，因此进行了修改。
4. 注释掉： PYTHON_LIB := /usr/lib
5. 取消注释： PYTHON_LIB := $(ANACONDA_HOME)/lib
6. 取消注释： WITH_PYTHON_LAYER :=1
7. 取消注释： USE_PKG_CONFIG :=1
8. 取消注释： OPENCV_VERSION :=3
9. 
```
## 编译和测试
1. 编译caffe
```
cd ~/caffe
make all -j8 #8个线程编译
make pycaffe
```
2. 修改环境变量
在文件~/.bashrc中添加如下：
```
export CAFFE_ROOT=/home/<username>/caffe/
export PYTHONPATH=/home/<username>/caffe/python:$PYTHONPATH
```
source ~/.bashrc更新环境变量
3. 测试caffe。在安装caffe以前，需要测试caffe是否能正常工作。
```
make test
make runtest
make distribute
```

## 安装中遇到的问题

问题1：
_caffe.so: undefined symbol: _ZN5boost6python6detail11init_moduleER11PyModuleDefPFvvE
方法：
1. 去掉注释：WITH_PYTHON_LAYER :=1
2. 修改python boost：PYTHON_LIBRARIES := boost_python-py34 
pytho boost默认使用python2。在路径/usr/lib/x86_64-linux-gnu/会有多个python boost，选择一个合适的。（可以使用”find . -name libboost_python*“罗列所有的python boost文件）

问题2：
Makefile.config中选择atlas。编译正常，测试正常。但是import caffe时出现如下错误：Intel MKL ERROR: Cannot load libmkl_avx2.so or libmkl_def.so.
方法：

问题3：
fatal error: caffe/proto/caffe.pb.h: No such file or directory
方法：
需要使用==protoc==手动生成==caffe.pb.h==:
```language
# In the directory you installed caffe to
protoc src/caffe/proto/caffe.proto --cpp_out=.
mkdir include/caffe/proto
mv src/caffe/proto/caffe.pb.h include/caffe/proto
```

问题4：
caffe fails with error undefined reference to 'cv::read(cv::String const&, int)' and undefined reference to 'cv::imdecode(cv::_InputArray const &, int)'
方法：
使用opencv3，在Makefile中没有把注释打开。

问题5：
libopencv_imgcodecs.so:undefined reference to "png_set_compression_level@PNG16_0'
方法：
```language
cd /usr/lib/x86_64-linux-gnu
sudo ln -s ~/anaconda3/envs/py27/lib/libpng16.so.16 libpng16.so.16
sudo ldconfig
```
问题1：
make pycaffe时找不到python.h文件。
方法：
使用如下语句编译pycaffe:"CPATH=/home/zhangkj/anaconda/include/python3.6m/ make pycaffe"
问题2：
.build_release/tools/caffe: error while loading shared libraries: libhdf5_hl.so.10: cannot open shared object file: No such file or directory
方法：
该问题多出现在使用anaconda安INST装Python包。可以在～/.bashrc中添加
“export LD_LIBRARY_PATH=/home/zhangkj/anaconda/lib:$LD_LIBRARY_PATH"

问题3：
fatal error: pyconfig.h: No such file or directory
方法：
$ export CPLUS_INCLUDE_PATH=~/software/anaconda3/envs/py27/include/python2.7