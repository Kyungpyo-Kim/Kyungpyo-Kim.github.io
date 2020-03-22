---
title: Setup - Ubuntu18.04 + Cuda10.0 + cuDNN7.4.2 + TF1.14.0
categories: study
modified: 
tags: [AI, ubuntu]
ads: true
excerpt:
image:
  feature:
  teaser: teaser/400x250.jpg
  thumb:
sitemap :
  changefreq : daily
  priority : 1.0
toc: true
toc_label: "Table of Contents"
toc_icon: "cog" 
---

## AI, Deep Learning 환경 준비하기

### My PC Info
* Ubuntu18.04
* NVIDIA GTX1650

CUDA 및 cuDNN 을 설치 할 때, GPU와 호환 가능한 버전을 반드시 체크해야 한다. 그리고 Tensorflow 도 마찬가지로 CUDA와의 호환성을 체크하여야 한다. 현재(2020-03-22) Tensorflow 는 CUDA10.0(not 10.2)까지만 호환 가능하다.
{: .notice}

### CUDA 10.0 Installation

설치 파일은 NVIDIA 홈페이지에서 다운로드 가능하다.

**install**
```bash
sudo cuda_10.0.130_410.48_linux.run
$ export PATH=/usr/local/cuda-10.0/bin:$PATH
$ export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
```

### cuDNN 7.4.2 Installation

설치 파일은 NVIDIA 홈페이지에서 다운로드 가능하다.

**install**
```bash
sudo dpkg -i ./libcudnn7-dev_7.4.2.24-1+cuda10.0_amd64.deb 
sudo dpkg -i ./libcudnn7-doc_7.4.2.24-1+cuda10.0_amd64.deb 
sudo dpkg -i ./libcudnn7_7.4.2.24-1+cuda10.0_amd64.deb 
```

### CUPTI

[**CUPTI**](https://docs.nvidia.com/cuda/cupti/index.html)
The CUDA Profiling Tools Interface (CUPTI) enables the creation of profiling and tracing tools that target CUDA applications.

CUPTI provides the following APIs:
the Activity API,
the Callback API,
the Event API,
the Metric API, and
the Profiler API.
Using these APIs, you can develop profiling tools that give insight into the CPU and GPU behavior of CUDA applications.

CUPTI is delivered as a dynamic library on all platforms supported by CUDA.

**install**
```bash
sudo apt install libcupti-dev -y
```

### Tensorflow-gpu 1.14

**install**
```
pip3 install tensorflow-gpu opencv-python jupyter matplotlib tqdm scipy googledrivedownloader
pip3 uninstall numpy
pip3 install --upgrade numpy==1.16.1
```

**test**
```python
>>> import tensorflow as tf
>>> hello = tf.constant(‘Hello, TensorFlow!’)
>>> sess = tf.Session()
>>> sess.run(hello)
b’Hello, TensorFlow!’
```
