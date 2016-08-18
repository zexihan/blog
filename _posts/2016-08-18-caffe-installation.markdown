---
layout:     post
title:      "Caffe Installation on Ubuntu"
subtitle:   "Caffe"
date:       2016-08-18
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Caffe
    - Installation
---

During my deep learning internship in CASIA, I did almost all the experiments on Caffe platform. Caffe is a deep learning framework with expressive architerture, extensible code, high speed and a muture community. So almost all the PhDs and graduate students (I was an undergraduate at that time) in the National Laboratory of Pattern Recognition (NLPR) took Caffe as the first choice for their developments.

The problem is that although it is an excellent framework, the installation process is kind of sophisticated especially for a novice. At the startup stage of my internship, I spent two weeks on the installation. Because I was not familiar with the operations of Linux operating system, and the version of Ubuntu of the lab's computer is 12.04 which means a lot of dependences need to be updated, however I was not given the root authority for the lab's computer so I had to face up with more complicated procedure in the installation of those dependences. Jesus T^T

Fortunately, you do not have to be like me facing so many difficulties as long as you follow my instructions below. :) 

## Ubuntu Installation

### Before Caffe

Before the installation, I strongly suggest your computer equiped with a brand new Ubuntu system with version 14.04 or 16.04 LTS, and you should get the root authority of your system, which will avoid a lot of troubles.

Besides I recommend you install the latest version of MATLAB and Python correctly on your Ubuntu before installing Caffe for extensive development.

 First, open the terminal and type the following codes to keep your software updated on your Ubuntu.

```
$ sudo su
$ apt-get update
```

Then, let's do it!

### Download Caffe

Download Caffe from its official Github page: https://github.com/BVLC/caffe

Unzip it to your home directory for your convenience.

### General dependencies of Caffe

```
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
$ sudo apt-get install --no-install-recommends libboost-all-dev
```
### CUDA: 

Install via the NVIDIA package instead of apt-get to be certain of the library and driver versions. Install the library and latest driver separately; the driver bundled with the library is usually out-of-date. This can be skipped for CPU-only installation.

### BLAS: 

install ATLAS by 
```
$ sudo apt-get install libatlas-base-dev
```

or install OpenBLAS or MKL for better CPU performance.

Python (optional): if you use the default Python you will need to sudo apt-get install the python-dev package to have the Python headers for building the pycaffe interface.

### Remaining dependencies, 14.04

Everything is packaged in 14.04.

```
$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```

## Compilation

Caffe can be compiled with Make.

### Compilation with Make

Configure the build by copying and modifying the example Makefile.config for your setup. The defaults should work, but uncomment the relevant lines if using Anaconda Python.

```
$ cp Makefile.config.example Makefile.config # Adjust Makefile.config (for example, if using Anaconda Python, or if cuDNN is desired)
$ make all -j8
$ make test
$ make runtest
```

* For CPU & GPU accelerated Caffe, no changes are needed.
* For cuDNN acceleration using NVIDIA’s proprietary cuDNN software, uncomment the USE_CUDNN := 1 switch in Makefile.config. cuDNN is sometimes but not always faster than Caffe’s GPU acceleration.
* For CPU-only Caffe, uncomment CPU_ONLY := 1 in Makefile.config.

To compile the Python and MATLAB wrappers do 
```
$ make pycaffe 
```
and 
```
$ make matcaffe 
```
respectively at the root directory of Caffe. Be sure to set your MATLAB and Python paths in Makefile.config first!

Now that you have installed Caffe, check out the MNIST tutorial and the reference ImageNet model tutorial on the official website.
