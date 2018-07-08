---
layout: post
title: 논문리뷰-Accurate Single Detector Using Reccurrent Rolling Convolution (RRC)
comments: true
category: Deep Learning
tags:
- ML
- Object Detection
- RNN
- Deeplearning
- Computer Vision
- Paper review
---

![Paper_head]({{site.url}}/images/Accurate-Single-Detector-Using-Reccurrent-Rolling-Convolution/paper_header.png){:.aligncenter}



_주관적인 의견은 이탤릭체로 표기하였습니다._





# Introduction



<p align="justify">

현재 성능이 좋은 Object Detection 시스템들은 주로 R-CNN의 2-stage 계열의 변형들입니다.  R-CNN(2-stage 계열)은 1. object가 있을 법한 위치를 제안하는 Region Proposal Module 2. 해당 위치에서의 class를 찾는 Classification Module이 분리되어있습니다. 그 외의 다른 줄기의 대표적인 Object Detection System 중에는 single stage Object Detection System이 있습니다. single stage object detection system은 구현하기 간단하고 연산효율도 좋지만 2-stage 계열의 object detection system과 비교했을 때, IOU threshold 가 높게 설정된 benchmark성능에서 mAP 측면에서 경쟁이 되지 않습니다. 

<br/><br/>

</p>

**본 논문에서는 이를 해결하기 위해 "deep in context"를 고려한 Recurrent Rolling Convolution (RRC) 아키텍쳐를 제안합니다.**

<p align="justify">

<br/>

해당 논문의 Contribution은 다음과 같습니다.

</p>

* 높은 localization 퀄리티를 갖는 single detector를 학습시킬 수 있음을 증명했다.
* single stage detector의 성능 개선을 위한 키포인트는  반복적으로 context정보를 주입해서 bounding box regression 하는 것임을 발견했다.

# Motivation

<p align="justify">

<br/><br/>

<br/>

저자들은 기존의 object detection system들이  context(multi-scale 정보나, feature 주변의 occlusion이 일어난 지역)를 고려해서 정확한 bounding box의 위치를 regression해야하는데, 기존까지의 object system들은 그런 부분들을 고려하지 못했기 때문에 작은 object들이나 겹쳐진 object들이 영상에서 나타나게 되면 낮은 퀄리티의 bounding box들이 나타나는 것이라고 주장합니다. 

<br/>

<br/>

</p>



![localization_fail]({{site.url}}/images/Accurate-Single-Detector-Using-Reccurrent-Rolling-Convolution/localization_fail.png){: width="100%", height="100%"}



<p align="justify">



</p>