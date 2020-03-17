---
title: Jetson-nano 로 semantic segmentation
categories: study
modified: 
tags: [AI, Jetson, python]
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

## Jetson-nano 로 semantic segmentation 해보기
* Jetson nano 에 semantic segmentation 을 올려보기
  - Tensorflow 및 TensorRT 이용해보기
* 모델을 직접 만들 수도, 혹은 있는거를 그대로 올려볼 수 도 있다.
  - ICNet 을 Tensorflow 로 구현한 모델을 돌려보기
* 개발 언어는 모델을 직접 구현할 경우 python 을 이용하려고 한다.


### My PC Info
* Ubuntu18.04
* NVIDIA GTX1650

### CUDA 10.0 Installation
```bash
sudo cuda_10.0.130_410.48_linux.run
$ export PATH=/usr/local/cuda-10.0/bin:$PATH
$ export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
```
### cuDNN 7.4.2 Installation
```bash
sudo dpkg -i ./libcudnn7-dev_7.4.2.24-1+cuda10.0_amd64.deb 
sudo dpkg -i ./libcudnn7-doc_7.4.2.24-1+cuda10.0_amd64.deb 
sudo dpkg -i ./libcudnn7_7.4.2.24-1+cuda10.0_amd64.deb 
```
### cupti
```bash
sudo apt install libcupti-dev -y
```

### Tensorflow-gpu 1.14
```
pip3 install tensorflow-gpu opencv-python jupyter matplotlib tqdm scipy googledrivedownloader
pip3 uninstall numpy
pip3 install --upgrade numpy==1.16.1
```

```python
>>> import tensorflow as tf
>>> hello = tf.constant(‘Hello, TensorFlow!’)
>>> sess = tf.Session()
>>> sess.run(hello)
b’Hello, TensorFlow!’
```

### Demo results: 
Test inference speed
Average time: 0.5310, about 1.883186 fps

돌려본 결과, 2fps 정도 나온다...
1. 성능이 좀 떨어지더라도 모델의 속도를 높일 수 있는 방법 알아보기
2. Segnet Tensorflow 버전 돌려보기

## Tensorflow model 만들기
ICNet 돌려보기: https://github.com/hellochick/ICNet-tensorflow

## TensorRT 로 최적화하기

## Jetson nano 에서 돌려보기
