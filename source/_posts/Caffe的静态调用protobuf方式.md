---
title: Caffe使用问题解决之protobuf的静态调用
---

------

> * 问题来源
> * 问题原因
> * 解决办法
> * 参考资源

<!--more-->
## 问题来源
在Linux系统中使用Matlab的Faster RCNN做目标检测时，想必大家都会对于这样一个问题表示深恶痛绝：启动Matlab运行Faster RCNN的demo或者其它训练脚本后，如果不是正常结束运行（比如matlab代码出bug，或者调试过程中止），再次运行该matlab脚本时，会出现如下报错：
```python
# errors in Matlab:
[libprotobuf ERROR google/protobuf/descriptor_database.cc:57] File already exists in database: caffe.proto
[libprotobuf FATAL google/protobuf/descriptor.cc:954] CHECK failed: generated_database_->Add(encoded_file_descriptor, size): 

# errors in terminal:
Caught "std::exception" Exception message is:
CHECK failed: generated_database_->Add(encoded_file_descriptor, size): 

```

这个问题不仅仅是Matlab版的Faster RCNN中会出现，而且是在官方的BVLC/caffe中也同样存在，只要有多个程序同时运行时调用caffe，就会报上述`libprotobuf ERROR`错误。

## 问题原因
原因是因为在Linux操作系统中，以Ubuntu14.04为例，在配置caffe的系统环境时需要安装Google的protobuf库，官网给出的安装方式为：
```python
sudo apt-get install libprotobuf-dev protobuf-compiler
```
这样安装默认的protobuf的configure选项是生成动态链接库（学过一点C/C++的应该都知道动态库和静态库），即libprotobuf.so文件，无法满足同一个库文件被多个进程同时调用的要求，因此需要改成静态调用protobuf，也就是安装protobuf时需要生成动态链接库libprotobuf.a文件，然后在caffe里面以静态调用的方式使用protobuf。

## 解决办法
### protobuf的编译安装
以protobuf-2.5.0版本为例，首先从google官网下载protobuf源码包:
下载地址：[http://protobuf.googlecode.com/files/protobuf-2.3.0.zip](http://protobuf.googlecode.com/files/protobuf-2.3.0.zip).
附赠网盘地址：链接：[http://pan.baidu.com/s/1eSnHBEy](http://pan.baidu.com/s/1eSnHBEy) 密码：skrc

然后解压，修改配置文件
```python
sudo tar zxvf protobuf-2.5.0.tar.gz
cd protobuf-2.5.0
sudo gedit ./configure # 或者 sudo vim ./configure
```

然后查找到./configure文件的CFLAGS和CXXFLAGS，添加`-fPIC`选项，即做如下更改：
```
if test "x${ac_cv_env_CFLAGS_set}" = "x"; then :
	CFLAGS="-fPIC" 
fi 
if test "x${ac_cv_env_CXXFLAGS_set}" = "x"; then :   
	CXXFLAGS="-fPIC"
fi
```

然后添加`--disable-shared`选项进行protobuf的编译和安装：
```python
sudo sh ./configrue --disable-shared
sudo make -j8
sudo make check -j8
sudo make install -j8
```

最后确保上述命令输出没有错误后，刷新系统的共享库，并检查protobuf是否安装到系统目录：
```python
sudo ldconfig # refresh shared library cache  
protoc --version
```

### Caffe的编译前更改
在按照官网给的caffe编译命令之前，需要更改Makefile文件，以支持protobuf的动态加载
首先，命令```sudo gedit Makefile```打开Makefile文件，查找`LDFLAGS`的定义位置，添加如下一行选项：
```python
LDFLAGS += -Wl,-Bstatic -lprotobuf -Wl,-Bdynamic  
```

然后，查找`LIBRARIES`的定义位置，将`protobuf`一项删除，删除后的`LIBRARIES`定义为：
```python
LIBRARIES += glog gflags boost_system boost_filesystem m hdf5_hl hdf5
```

最后，查找`CXXFLAGS`的定义位置，添加`-fPIC`选项
```python
CXXFLAGS += -MMD -MP -fPIC
```

###Caffe的编译
```python
sudo make clean # if failed before
sudo make -j8
sudo make matcaffe
```

## 参考资源
[1]. [http://blog.csdn.net/fengbingchun/article/details/72318839](http://blog.csdn.net/fengbingchun/article/details/72318839)
[2]. [http://blog.csdn.net/linyushan11/article/details/10378419](http://blog.csdn.net/linyushan11/article/details/10378419)
[3]. [http://caffe.berkeleyvision.org/install_apt.html](http://caffe.berkeleyvision.org/install_apt.html)
[4]. [https://github.com/BVLC/caffe/issues/1917](https://github.com/BVLC/caffe/issues/1917)


