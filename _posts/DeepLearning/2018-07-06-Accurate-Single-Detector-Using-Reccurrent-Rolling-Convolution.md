---
layout: post
title: 논문리뷰-Accurate Single Detector Using Recurrent Rolling Convolution (RRC)-Done yet
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



# Summary

본 논문의 요약은 다음과 같습니다.



* 기존의 Object Detection System들은 IOU > 0.7 이상으로 평가시, 성능이 급격히 떨어진다.
* 위와 같은 성능 하락의 이유를 small objects, overlapping objects, occlusion때문으로 생각했다.
* Context(multi-scale, occluded region) 정보를 충분히 이용한다면 개선시킬 수 있을 것이다.
* 이러한 Context 정보를 활용하기 위해 각각의 feature map들이 Network의 전체 feature들을 공유할 수 있는 아래 그림과 같은 Network를 설계했다. (Recurrent Rolling Convolution)
* 그 결과 기존 Object Detection System들보다 성능이 월등히 좋았다.



![Network Architecture]({{site.url}}/images/Accurate-Single-Detector-Using-Reccurrent-Rolling-Convolution/Network_architecture.png){:.aligncenter}

<p align="center">

[Recurrent rolling convolution Network Architecture]

</p>

![Result Visualization]({{site.url}}/images/Accurate-Single-Detector-Using-Reccurrent-Rolling-Convolution/result_visualization.png){: width="100%", height="100%"}

<p align="center">

[compare between SSD and RRC(Recurrent Rolling Convolution)]

</p>



-------------------------------------------



# Introduction

* 성능이 좋은 Object Detection 시스템들은 주로 R-CNN의 2-stage 계열의 변형이 있습니다.
  * 2-Stage Object Detection 시스템의 작동 방식은 다음과 같습니다.
    * object가 있을 법한 위치를 제안하는 Region Proposal 
    * 제안된 Region에서 Class 분석

<br/>

* 다른 Object Detection System(Single stage) 방식도 존재하는데, 다음과 같은 특성을 갖습니다.
  * 2-Stage계열의 pipe-line을 하나로 통합
  * 구현이 쉽고, 연산 효율이 좋음
  * 2-stage 계열에 비해 IOU threshold가 높게 설정된(IOU > 0.7) 벤치마킹에서는 mAP가 낮음



<br/>

**본 논문의 저자들은 이를 해결하기 위해 "deep in context"를 고려한 Recurrent Rolling Convolution (RRC) 아키텍쳐를 제안합니다.**

<br/>

해당 논문의 Contribution은 다음과 같습니다.

* 높은 localization 퀄리티를 갖는 single detector를 학습시킬 수 있음을 증명.
* single stage detector의 성능 개선을 위한 키포인트는  반복적으로 context정보를 주입해서 bounding box regression 하는 것임을 발견.

<br/>

----------------------------------



# Motivation

* 저자들은 IOU>0.7 벤치마킹에서 Object Detection System 성능이 떨어지는 이유는  context(multi-scale 정보나, feature 주변의 occlusion이 일어난 지역)가 고려되지 못한 이유가 주 원인이라고 주장합니다.
* [End-to-end people detection in crowded scenes](https://arxiv.org/pdf/1506.04878.pdf)에서는 LSTM을 사용하여 sequence 데이터에서 detection을 적용했으나, Network는 sequence데이터에 의존적이므로 "Shallow in context"를 가진다고 이야기합니다.



![localization_fail]({{site.url}}/images/Accurate-Single-Detector-Using-Reccurrent-Rolling-Convolution/localization_fail.png){: width="100%", height="100%"}

<p align="center">

[failure localization in object detection system]

</p>

<p align="justify">



</p>

--------------------



# Analysis 

저자들은 Object Detection System에서 요구되는 조건과 지금 Convolution Neural Network가 갖는 한계점에 대해서 다음과 같이 언급합니다.



* robust한 object detection system은 multi-scale에서도 object detection을 잘 수행해야함

* CNN자체가 feature map의 해상도를 낮추는 효과가 있어, small object를 검출하기 어려움

  

저자들은 위에서 언급했던 multi-scale문제를 해결하고, 속도도 빠른 object detection system 중 SSD라는 모델을 선택하곤 현재 Object Detection System이 갖는 문제점과 한계점을 분석하고자 합니다. 

_아마도 저자들이 SSD를 선택한 이유는, SSD가 Multi Scale Issue를 해결했음에도 Object Detection System 중에서 art-of-art인 System이 아니기 때문에 이를 분석하는 것을 통해서 Single stage object Detection System의 문제를 파악하고 싶었던건 아닐까 생각하고 있습니다._

<br/>

## 1. Compare between SSD with 2-stage model

SSD 모델을 수학적으로 표현하면 다음과 같이 표현이 가능합니다.



$$ \Phi_n = f_n(\Phi_n-1) = f_n(f_n-1(... f_1(I))) $$ &emsp;&emsp;(1)

$$ Detection = D(\tau_n(\Phi_n), ..., \tau_n-k(\Phi_n-k)), n>K>0 $$&emsp;&emsp;(2) 

<br/>

$$ \Phi : Feature \, Map $$.

$$ f_n : Neural \, Network $$.

$$ D : Final \, Detection \, function $$.

$$ \tau : detection \, in \, specific \, scale $$.

<br/>



이는 아래의 그림과 같이, SSD가 각각의 scale에 대한 detection을 한다는 것을 $$ \tau_i(\Phi_i) $$로 볼 수 있고, 최종 Detection 출력값들은 모두를 고려하기 때문에 다음과 같다고 볼 수 있습니다. 



$$ Detection = D(\tau_n(\Phi_n), ..., \tau_n-k(\Phi_n-k)) $$

<br/>



![SSD]({{site.url}}/images/Accurate-Single-Detector-Using-Reccurrent-Rolling-Convolution/ssd.png){: width="100%", height="100%"}

<p align="center">

[SSD: Single Shot MultiBox Detector Architecture]

</p>

<br/>

식 $$ (2) $$을 따를 때, 우리는 Object Detection System에 대해서 다음과 같은 가정을 확인 할 수 있습니다.

<br/>

**모든 $$ \Phi $$는 정확한 detection과 localization을 할 정도로 충분히 정교(sophisticate)해야한다.**



여기서 feature map($$ \Phi $$)이 정교(sophisticate)하다는 말은 다음과 같습니다.

1. object의 detail을 살릴 수 있을 정도의 해상력(resolution)을 가지고 있어야 한다.
2. Network가 충분히 깊어서 고차원의 특징을 추출 할 수 있어야 한다.
3. Feature map은 적합안 contextual information을 포함하고 있어야한다.

<br/>

SSD 모델의 수식을 살펴보면, SSD 모델은 $$ (1) $$은 만족하나, 가정 $$ (2) $$는 만족하지 않습니다. Scale이 크면 큰 Feature일수록 Network의 깊이는 얉아(Shallow Network)이기 때문에, network가 충분히 깊어서 고차원의 특징을 추출 할수 있다고 볼수가 없다는 것입니다.



<br/>

Faster R-CNN의 경우에는 이러한 가정 $$ (2) $$에 대한 문제는 없습니다. 2-stage 계열의 Object Detection System을 수식으로 풀어보면, 다음과 같습니다.

<br/>

$$ Region proposals = \Re(\tau_n(\Phi_n)), n>0 $$

$$ \Re : Region proposal area$$&emsp;

<br/>

하지만 2-stage Object Detection System은 가정 $$ (1) $$을 충족시키지 못합니다. 이렇듯 현재 기존의 Object Detection System들은 제시된 가정 $$ (1), (2), (3) $$을 충족하지 않습니다.



<_이 부분에 대해서는 조금 애매한 부분이 있는 것 같습니다. 만약에 (1)의 조건이 Multi-scale에 대한 feature map(resolution)을 의미한다면, Region Proposal Network가 Multi-Scale을 뽑지 않기 때문에 문제가 되지만, Region Proposal Network가 CNN으로 구성되어있고 충분히 깊지 않다면 Region Proposal 자체도 강력하지 않을 가능성이 있는 문제가 있습니다. 이를 단순히 (1)의 조건 "the feature map should have enough resolution to represent the fine details of the object"을 충족하지 않는다라고 일반화하기에는 무리가 있는 거 같습니다. 그렇다면, 단순히 2-stage model에서 Region Proposal Network 부분을 RRC로 대체해서 변형된 RRC based Region Proposal Network를 통해 Region을 제안받고, 충분히 깊은 CNN Network를 통해서 Classification을 한다면 꼭 Single Stage냐 2-stage냐의 차이는 무엇이 되는지 모르겠습니다. RRC 자체가 Single-stage model이라고 이야기하지만, 우리가 Single Stage Object Detection System을 사용하는 이유는 학습이 편한것도 있으나, 속도적인 측면에서 이점을 보려고 사용하는 목적이 큰듯한데, 이렇게 Recurrent method를 쓰게되면 Single Stage의 이점을 모두 날려버리는 형태가 갖는 이점은 무엇일까요?_>

<br/>

## Solution

위에서 설명한 것과 같이 Single Stage / 2-Stage model 모두가 3가지의 요건을 충족하지 못하기 때문에, 저자들은 다음과 같은 새로운 Object Detection system 프레임워크를 제안합니다.

<br/>

$ eq. (4)$

$$ Detection = \hat{D}(\tau_{n}(\hat{\Phi}_{n}(H)), \tau_{n-1}(\hat{\Phi}_{n-1}(H)), ... \tau_{n-k}(\hat{\Phi}_{n-k}(H))) \qquad \, $$

$$ H = (\Phi_{n}, \Phi_{n-1}, ... ,\Phi_{n-k}) \qquad  n>k>0 $$ &ensp; 

$$ size(\Phi_{n-k}) = size(\hat{\Phi}_{n-k}(H)),  \, \forall{k} $$ &ensp;



<br/>

여기서 $H$는 Network에서 얻은 모든 feature map들을 포함하는 셋이며, 이러한 Feature $H$는 식(2)에서 언급되었던 Detection function $D(\cdot)$에 들어가게됩니다.



$\hat{\Phi}_{n}(\cdot)$의 경우에는 식(2)와는 다르게, 특정한 layer의 feature만을 표현하는 것이 아니라, network 전체에서 출력되는 feature map이 고려된 새로운 Feature Map에 대한 표현으로 변경되게 됩니다.



