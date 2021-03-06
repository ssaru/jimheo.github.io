---
layout: post
title: Introducing Kubeflow
comments: true
category: MLOps
tags:
- MLOps
- Cloud
- Deep learning
- Kubernets
- Serving
- Deploy
---

## 머신러닝(Machine learning)에서 3가지 도전

​    

### Composability



일반적으로 머신러닝(Machine learning)이나 딥러닝(Deep learning)을 하는 사람들은 모델을 먼저 만들게 되는데, 이때 다수의 사람들이 데이터 사이언티스트(Data scientists)의 작업을 쉽게 도와주는 텐서플로우(Tensorflow), 카페(Caffe), 사이킷 런(Scikit Learn) 등의 프레임워크(Framework)를 사용하게된다. 

하지만 실제 상품화 레벨의 솔루션(production-grade solution)을 만들려고할 때는 굉장히 복잡한 단계를 거치게된다. 여기에는 Importing, Transforming, Visualizing the data, building / validating the model, training the model at scale, deploying the model to production 등이 포함된다.

![kubeflow_compostability](https://user-images.githubusercontent.com/13328380/50627718-2f907800-0f78-11e9-98c4-cd51bed9434c.png)

<center>
[Complex step] 
</center>

​    

### Portability

VMware의 파운더이자 CTO인 Joe Beda는 `dev/staging/prod 사이의 차이점은 점점 줄어들 것`이라고 했다. 

머신러닝(Machine learning)의 다른 스텝들은 종종 완전하게 다른 시스템안에 속한다. 거기다 하드웨어나, 가속기, OS같은 로우-레벨(lower-level) 컴포넌트(Component)들도 고려되어야하기 때문에 복잡성이 더 커진다. 

따라서 자동화된 시스템이나 툴이 없다면 이러한 요소들의 변화가 빠르게 커지고 관리하기 어려워질 수 있다. 또한 반복된 실험으로부터 나온 일관된 결과를 얻기도 매우 어려워진다.



![defferent_step](https://user-images.githubusercontent.com/13328380/50628452-83e92700-0f7b-11e9-929c-d5feef3be0b2.png)

<center>
[entirely different system] 
</center>

​    

### Scalability

머신러닝(Machine learning)에서 최근 가장 큰 돌파구는 클라우드(cloud)에서 대규모 스케일(scale)과 수용력(capacity)을 갖는 결과를 얻은 것이다. 해당 결과는 다양한 머신의 종류(Machine type)들과 하드웨어에 관련된 가속기(그래픽 카드 / 텐서 연산 유닛)을 포함한다. 게다가 scalability는 하드웨어나 소프트웨어에만 해당하는 것이 아니다. scalability는 협업을 통해 팀을 확장하고 다수의 실험을 단순화할 수 있는 것도 중요하다.

​    

### Kubernetes and machine learning

쿠베르네티스(Kubernets)는 어디에서나 복잡한 워크로드 빠르게 배포가 가능한 솔루션으로 빠르게 성장했다.  쿠베르네티스가 간단한 stateless service에서 시작하는 동안 고객들은 쿠베르네티스가 제공하는 풍부한 API, 안정성, 성능을 이용하여 복잡한 워크로드를 갖는 플랫폼으로 이동했다. 머신러닝 커뮤니티는 쿠베르네티스의 핵심 이점을 활용하기 시작했다. 하지만 쿠베르네티스를 활용해서 서비스를 배포하는 것은 아직도 복잡하고 여러 벤더들을 섞고, 수작업으로 만져야하는 일들이 요구된다. 신중하고 정교한 셋업을 위해서 이러한 서비스들을 연결하고 관리하는 것은 모델을 연구하는 데이터 사이언티스에게 복잡한 장벽이다.

​    

## Intoducing Kubeflow

이러한 문제에 발맞춰서 Kubeflow 프로젝트가 2017년 말에 시작되었다. Kubelflow의 목적은 쿠베르네티스 위에서 돌고있는 머신러닝 프로젝트의 개발 / 배포 그리고  구성/이식성/확장성을 관리하는 것을 쉽게 만들어주는 것이다.



Kubeflow는 아래와 같은 기능을 지원한다.

- 협업과 인터렉티브 학습을 위한 JupyterHub
- Custom resource에서의 Tensorflow 학습
- Tensorflow 서빙 배포
- GitOps를 위한 Argo CD 지원
- non-Tensorflow Python model과 복잡한 추론을 위한 SeldonCore
- 리버스 프록시(Reverse proxy)
- 어디에서나 쿠베르네티스가 작동할 수 있게 연결

​    

## Reference

[[1] Kubeflow: Cloud-native machine learning with Kubernetes](https://opensource.com/article/18/6/kubeflow)
