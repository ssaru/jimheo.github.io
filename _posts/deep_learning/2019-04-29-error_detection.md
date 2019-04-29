---
layout: post
title: 논문리뷰-A Baseline For Detecting Misclassified And Out-Of-Distribution Examples In Neural Networks
comments: true
category: Deep Learning
tags:
- ML
- abnormal detection
- out of distribution
- Paper review
---

오랜만에 논문 리뷰

논문 양도 많지 않고, 깨작깨작 생각을 적어보려한다.

# Softmax high-confidence problem

- 클래스 분류 문제(classification)에서 많이 사용하는 소프트맥스(softmax) 수식은 아래와 같음.

  - $softmax(z_{i}) = \frac{exp(z_{i})}{\sum_{j}{exp(z_{j})}}$

- 소프트맥스(softmax) 확률은 빠르게 증가하는 지수함수(fast-growing exponential function)로 계산된다.

  - 입력값(input)의 작은 변화도 출력값(output)에 큰 영향을 준다.

- 소프트맥스(softmax)는 최대함수(max; 지시함수)함수를 완화제(smooth approximation)을 이용해서 매끄럽게 만든 함수이다. (해당 내용은 [여기](https://www.johndcook.com/blog/2010/01/13/soft-maximum/) 참조)

  - 따라서 학습한 데이터 분포 외(out of distribution)의 결과에서 균등분포(uniform distribution)을 관찰하는 것은 굉장히 드물다.
  - 소프트맥스 함수 자체가 본질적으로 지시함수이기 때문에 어떠한 최대값을 뽑게끔 만들어진 함수다.
    - 따라서 균등분포의 출력값을 뽑아낼 수가 없다.

- 소프트맥스(softmax) 분포로부터 구한 예측 확률(prediction probability)는 직접적인 신뢰도(confidence score)를 비교하기에는 굉장히 약한 성능(poor)을 보여준다.

  - 예를들어 MNIST를 학습했다고 가정하자

  - 네트워크 구조는

  - 이때, 숫자를 넣으면 높은 값이 나와야하며, 알파벳을 넣으면 낮은 값이 나와야한다.

    - 하지만 소프트맥스는 그렇지 못하다.

- 하지만, 학습 데이터 분포 외의 데이터(out of distribution)과 정확하지 않은 입력 데이터(incorrect input)의 예측 확률(prediction probabiity)는 정확한 입력값(correct input)값의 예측확률(prediction probability)보다는 낮게 나온다.

  - 정확한 입력값과, 부정확한 입력값 혹은 학습된 데이터 분포 외의 데이터에 대한 예측 확률의 통계적 성질을 이용하면 정상적이지 않은 데이터를 탐지할 수 있을 것이다.

<br/>

# Problem formulation and evaluation

- 본 논문에서는 AUROC(Area Under the Receiver Operating Characteristic curve)와 AUPR(Area Under the Precision-Recall curve)를 이용해서 성능 비교를 할 수 있으며, 비정상 데이터를 탐지할 수 있다고 함.

> 해당 내용은 추후 보강하겠음.

<br/>

# Softmax prediction probability as a baseline

- 핵심은 소프트맥스에서 최대 확률을 구하고, 데이터가 학습된 데이터 분포 내에 있는지 찾는 것이다.

- 테스트 데이터를 학습한 데이터 분포와, 학습하지 않은 데이터 분포를 분리한다.

- PR과 ROC를 구한다.

  - PR과 ROC는 모델의 성능을 나타내는 지표가 된다.

- 위와 같은 방법으로 여러 실험을 진행함

  - 실험 결과는 논문 참조.

<br/>

# Abnormality detection with auxiliary decoders


