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


### [ICNet](https://github.com/hellochick/ICNet-tensorflow) Demo results: 
Test inference speed
Average time: 0.5310, about 1.883186 fps

돌려본 결과, 2fps 정도 나온다...
1. 성능이 좀 떨어지더라도 모델의 속도를 높일 수 있는 방법 알아보기
2. Segnet Tensorflow 버전 돌려보기

### [Real-time Semantic Segmentation Comparative Study](https://github.com/MSiam/TFSegmentation) 
model training --> 수행 완료
model 돌려보기
  * 이미지 실행
  * 결과 overlap
  * fps 측정
    - x_batch shape:  (1, 512, 1024, 3)
      ```
      Statistics of the FPSMeter
      Frame per second: 5.80 fps
      Milliseconds per frame: 172.40 ms in one frame
      These statistics are calculated based on
      500 Frames and the whole taken time is 86.2022 Seconds
      ```

ICNet 보다는 훨씬 높은 FPS를 보여주었다.

Jetson Nano Cuda core: 128
Jetson TX2 Cuda core: 256
GTX1660 Cuda core: 896  

## TensorRT 로 최적화하기

## Jetson nano 에서 돌려보기
