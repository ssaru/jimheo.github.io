---
layout: post
title: Introduce & Overview Seldon Core
comments: true
category: MLOps
tags:
- MLOps
- ML
- Cloud
- Deep learning
- Kubernets
- Serving
- Deploy
---



[Seldon-Core](https://www.seldon.io)는 [Istio](https://istio.io/)와 함께 [KubeFlow](https://www.kubeflow.org/)에 통합되어있는 Serving Toolkit이다. [Istio](https://istio.io/)는 순수 Serving만 하기 때문에 [TensorFlow Serving](https://www.tensorflow.org/serving/)결합되어 사용할 수 밖에 없으며, 그 외의 딥러닝 프레임워크들은 [KubeFlow](https://www.kubeflow.org/) 내에서 [Seldon-Core](https://www.seldon.io)사용을 추천하고있다. 



이번에 다시한번 [KubeFlow](https://www.kubeflow.org/)을 설명할 때, 언급했던 머신러닝을 제품화하는데 있는 챌린지와 그에 따른 대응으로 [KubeFlow](https://www.kubeflow.org/)를 간략하게 소개하고, [Seldon-Core](https://www.seldon.io)에 대해서 설명하도록 하겠다. 

​    

# 머신러닝(Machine Learning)에서의 3가지 도전

과거까지(~2017년)는 잘 작동하고 좋은 Model를 만드는 것에 초점이 되어있었다. 

*정확히 이야기해서 초점이 되어있다라기보다 그 동안 수많은 딥러닝을 하는 회사 및 기관들이 모델을 만들면 어떻게 **배포**할지 우여곡절을 겪고 이를 해소하기 위해서 본격적으로 나섰다라고 보는게 맞지 않을까 싶다.*



![kubeflow-portable-and-scalable-machine-learning-using-jupyterhub-and-kubernetes-pydata-delhi-2018-3-638](https://user-images.githubusercontent.com/13328380/51166999-a9b1ec80-18e8-11e9-90ef-ee453ac3ad9e.jpg)



현재(2017년 ~ 현재)까지는 Machine Learning 모델을 배포하면서 겪었던 이슈사항들을 하나하나씩 정리하며 이를 어떻게 쉽게 만들것인가에 대한 고민을 시작하게 되었다. 이렇게 이슈를 정리하다보니까 실제로 만든 모델을 제품화하는 과정에서 좋은 모델을 작성하는 코드는 아주 작은 일부분이고 이를 위한 Infrastructure적인 요소가 더 중요하다는 것을 확인하게 된다.



![1 q3rjbataemk40nmu77jchq](https://user-images.githubusercontent.com/13328380/51166598-6f941b00-18e7-11e9-8fc1-3476edfbe2fb.png)



Machine Learning Service를 제품화하는 과정에서  챌린지는 다음과 같다.

- Composability
- Portability
- Scalability

    

## Composability

일반적으로 머신러닝(Machine learning)이나 딥러닝(Deep learning)을 하는 사람들은 모델을 먼저 만들게 되는데, 이때 다수의 사람들이 데이터 사이언티스트(Data scientists)의 작업을 쉽게 도와주는 텐서플로우(Tensorflow), 카페(Caffe), 사이킷 런(Scikit Learn) 등의 프레임워크(Framework)를 사용하게된다. 

하지만 실제 상품화 레벨의 솔루션(production-grade solution)을 만들려고할 때는 굉장히 복잡한 단계를 거치게된다. 여기에는 Importing, Transforming, Visualizing the data, building / validating the model, training the model at scale, deploying the model to production 등이 포함된다.



![kubeflow_compostability](https://user-images.githubusercontent.com/13328380/50627718-2f907800-0f78-11e9-98c4-cd51bed9434c.png)

<center>
[Complex step] 
</center>



- 배포는 모델 생성했다고 끝나는 것이 아니라, 엄청 복잡한 단계를 거친다.
- 이를 쉽게 조립할 수 있게 만들어줘야한다.



​    

## Portability

VMware의 파운더이자 CTO인 Joe Beda는 `dev/staging/prod 사이의 차이점은 점점 줄어들 것`이라고 했다. 

머신러닝(Machine learning)의 다른 스텝들은 종종 완전하게 다른 시스템안에 속한다. 거기다 하드웨어나, 가속기, OS같은 로우-레벨(lower-level) 컴포넌트(Component)들도 고려되어야하기 때문에 복잡성이 더 커진다. 

따라서 자동화된 시스템이나 툴이 없다면 이러한 요소들의 변화가 빠르게 커지고 관리하기 어려워질 수 있다. 또한 반복된 실험으로부터 나온 일관된 결과를 얻기도 매우 어려워진다.



![running-tensorflow-in-production-68-638](https://user-images.githubusercontent.com/13328380/51167287-889dcb80-18e9-11e9-9d11-d2d4c6952923.jpg)

<center>
[Portability with kubeflow] 
</center>



- 머신러닝의 배포 스텝들은 완전히 다른 시스템 안에 속하는 경우가 있으며, 로우-레벨 컴포넌트에 의존적이기 때문에 복잡도가 많이 커진다.
- 이렇게 복잡도가 커진 시스템을 다른 시스템에 쉽게 이식할 수 있게 만들어줘야한다.



​    

## Scalability

머신러닝(Machine learning)에서 최근 가장 큰 돌파구는 클라우드(cloud)에서 대규모 스케일(scale)과 수용력(capacity)을 갖는 결과를 얻은 것이다. 해당 결과는 다양한 머신의 종류(Machine type)들과 하드웨어에 관련된 가속기(그래픽 카드 / 텐서 연산 유닛)을 포함한다. 게다가 scalability는 하드웨어나 소프트웨어에만 해당하는 것이 아니다. scalability는 협업을 통해 팀을 확장하고 다수의 실험을 단순화할 수 있는 것도 중요하다.



![image2](https://user-images.githubusercontent.com/13328380/51167354-be42b480-18e9-11e9-9bbe-4513b3fe8abe.png)



- 실제 제품단계의 레벨에서는 대규모 트래픽 및 동시접속자가 발생하기 때문에 상황에 맞춰서 대규모 트래픽을 감당할 수 있을 정도로 Scale out / in 하는 유연하고 수용력이 강해야한다.

​    

# Kubeflow

쿠베르네티스(Kubernets)는 어디에서나 복잡한 워크로드 빠르게 배포가 가능한 솔루션으로 빠르게 성장했다.  쿠베르네티스가 간단한 stateless service에서 시작하는 동안 고객들은 쿠베르네티스가 제공하는 풍부한 API, 안정성, 성능을 이용하여 복잡한 워크로드를 갖는 플랫폼으로 이동했다. 머신러닝 커뮤니티는 쿠베르네티스의 핵심 이점을 활용하기 시작했다. 하지만 쿠베르네티스를 활용해서 서비스를 배포하는 것은 아직도 복잡하고 여러 벤더들을 섞고, 수작업으로 만져야하는 일들이 요구된다. 신중하고 정교한 셋업을 위해서 이러한 서비스들을 연결하고 관리하는 것은 모델을 연구하는 데이터 사이언티스에게 복잡한 장벽이다.



*개인적으로 생각했을 때, Kubeflow의 핵심 컴포넌트는 다음과 같다.*

- Ksonnet
- JupyterHub; jupyter notebook
- Distributed Training
  - Chainer
  - Pytorch
  - Tensorflow
  - mxnet
- Katib; Hyperparameter seacher
- ModelDB; management model history
- Pipeline
- Serving
  - Istio
  - **Seldon-Core**
- ArgoCD : (CD/CI) GitOps for kubeflow



아주 자세하진 않지만 대략적으로 kubeflow의 특징을 알고싶다면 내 이전 글 [Introducing feature of kubeflow](https://ssaru.github.io/mlops/2019/01/04/introduce_kubeflow_function.html)을 확인하자

​    

# Seldon-Core

Seldon-Core는 Kubeflow에서 Serving위해 사용되는 Toolkit이며, Serving을 지원하지 않는 딥러닝 프레임워크들이 Serving이 가능하도록 만들어주는 핵심 기능을 가지고 있다.

​    

## Challenge 

[Seldon-Core 깃헙 페이지](https://github.com/SeldonIO/seldon-core)에서는 Machine Learning Deployment에 대한 [challenges](https://github.com/SeldonIO/seldon-core/blob/master/docs/challenges.md)를 언급한다. Seldon-Core에서 언급된 Challenge는 다음과 같다.

- 쉬운 배포
- 그래프 실행 및 scale out/in, rolling update
- health check 및 failed components의 recovery 보장
- ML을 위한 Infrastructure 최적화
- Latency 최적화
- APIs / RESTful / gRPC를 이용한 비지니스 어플리케이션 연결
- 복잡한 런타임 micro service graphs 구성
  - request 라우팅
  - 데이터 변환
  - 앙상블 결과
- 다양한 개발방식 허용
  - 동기식
  - 비동기식
  - 배치
- 버전 정리 및 감사 허용
  - CI
  - CD
- 모니터링 제공
  - 기본적인 메트릭 : Accuracy, request latency, throughput
- 복잡한 메트릭
  - concept drift
  - bias detection
  - outlier detection
- 최적화 허용
  - AB Tests
  - Multi-Armed Bandits

    

## Goals

Seldon-Core는 이러한 Challenge를 풀기위해서 존재하는 Toolkit이며, 이를 이루기 위해 다음과 같은 High-Level Goal들은 다음과 같다.



- 데이터 사이언티스트가 machine learning toolkit 및 programming language를 이용하여 모델을 작성할 수 있게 함
  - Python
    - tensorflow
    - sklearn
  - Spark
  - H2O
  - R
- 배포시 자동으로 RESTful 및 gRPC을 사용하도록 설정
- micro service에 복잡한 runtime graph를 배포할 수 있음. 여기서 복잡한 runtime graph란 다음과 같이 구성될 수 있음
  - Models
  - Router
  - Combiners : 그래프의 응답에 대한 결함(앙상블 모델)
  - Transformers : request 혹은 response에 대한 변환
- 모델의 전체 라이프 사이클 관리
  - 가동중지시간(downtime)없이 runtime graph update(rolling update)
  - Scaling
  - Monitoring
  - Security

     

## Why it this important?

그러면 Seldon-Core의 이러한 특징이 중요한 이유는 무엇일까?

### 1. Efficiency

전통적인 infrastructure 스택 및 DevOps 프로세스는 ML 도메인에 잘 적용이 되지 않으며, 해당 도메인에 있어서 이를 해결할 OpenSource가 형성되어있지 않아서 제한적이다. 이로인해 기업들은 막대한 비용을 들여서 자체 서비스를 구축하거나, 독점적인 서비스를 이용한다.



또한 DevOps와 ML에 걸쳐서 다양한 스킬셋을 가지고있는 데이터 엔지니어의 수는 극히 적다. 이러한 비효율적인 구조는 데이터 사이언티스트가 서비스 및 품질문제에 몰입하게 만들게 되며 이로인해 데이터 사이언티스트가 집중해야할 영역(더 나은 모델을 만드는 것)에서 멀어지게 만든다.

​     

### 2. Innovation

모델이 실제 Production level에 있는 경우에만 실제 문제에 대한 모델의 성능을 측정할 수 있다. Seldon-Core는 데이터 과학분야를 application release cycle로 부터 분리하여 반복 주기를 더 가속화할 수 있다. AB 테스트나, Multi-Armed Bandit와 같은 실험 프로세스는 종종 기계학습을 위해 디자인되지 않은 툴을 사용하기 때문에 일반적으로 스택에서 굉장히 높은 곳에 위치하고 있다. inference graph와 모델들의 촘촘한 연결은 반복주기를 가속화하며, 유즈 케이스를 신속하게 구축/최적화 하여 혁신과 ROI를 가속화할 수 있다.

​    

### 3. Freedom

IDC 2017의 연구에 따르면, 상업적인 혹은 백업, 탄력성, 규제와 같은 이유/목적으로 인해서 40%의 유럽조직이 application을 클라우드로 확장하고 있다.  단일 클라우드 ML 시스템에 종속적이지 않은 플랫폼을 사용하면, 다양한 ML 클라우드 시스템 사용을 촉진할 수 있다. 또한 Tensorflow와 같은 DL을 위한 프레임워크가 지속적으로 나오고 있기 때문에 더 빠른 모델 개발을 위해 프레임워크에 종속되지 않는 플랫폼이 필요하다.

​    

## Introduce Seldon-Core

이제 대략적인 Seldon-Core의 목적에 대해서 알아봤으니, 조금 더 자세히 들어가보자.



Seldon-Core는 회사들의 ML/DL 프로젝트의 마지막 단계 - (Model을 Production에 넣어서 실제 문제를 풀수있게 만듬)를 해결하는데 초점을 둔다.



이를 통해서 데이터 사이언티스트들은 더 나은 모델을 만들기 위해서 집중하고, 개발자들은 그들이 이해한 툴을 이용해서 조금 더 효과적으로 ML Solution을 배포하고 관리할 수 있다.



Seldon-Core 플랫폼의 목적은 다음과 같다.

- 데이터 사이언티스트가 ML toolkit이나 Programming language를 이용해서 만들어진 모델을 배포할 수 있게 한다. 초기에서는 H20, Spark, Tensorflow나 Scikit-learn과 같은 Python 기반의 도구/언어들을 지원한다.
- 배포시에 ML/DL 프로젝트들이  REST / gRPC에 노출하여 예측이 필요한 비지니스 서비스 및 프로그램에 쉽게 통합할 수 있게 한다.
- 가동중지 없이 update runtime graph, scaling, monitoring, security를 포함해서 배포된 모델의 전체 라이프사이클 관리를 핸들링한다.

​    

## Overview Seldon-Core

Seldon Core은 아래의 그림과 같이 3단계로 작업이 진행된다.

아래와 같이 진행을 하면 곧바로 Serving 모드로 들어갈 수 있으며 이를 설정하는 방법 자체가 굉장히 잘 추상화되어있어서 쉽게 쓸 수 있게 되어있다.



- Seldon-Core 설치
- 내부 Seldon Microservice API에 맞게 runtime ML을 Docker Image로 Packaging(S2I 이용)
- ksonnet나 helm을 이용하여 runtime service graph를 Seldon Deployment resource로 정의하고, 배포



![steps](https://user-images.githubusercontent.com/13328380/50888497-23556080-1439-11e9-9a8e-92d323cba094.png)



자 이제 간단하게 Seldon-Core가 왜 만들어졌고, 무엇을 해결하려고하는지, 그리고 어떤방식으로 사용하는지 거시적인 컨셉을 이해했다면 다음 글을 통해서 하나하나씩 과정에 대해서 뜯어보자



*(하지만 다음글은... 언제일지 장담할 수 없다....)*

​    

# Reference

[[1]. Kubernetized Machine Learning and AI Using KubeFlow](https://mapr.com/blog/kubernetized-machine-learning-and-ai-using-kubeflow/)

[[2]. Running Tensorflow in Production](https://www.slideshare.net/matthiasfeys/running-tensorflow-in-production)

[[3]. Kubeflow: portable and scalable machine learning using Jupyterhub and Kubernetes[PyData Delhi 2018]](https://www.slideshare.net/AkashTandon7/kubeflow-portable-and-scalable-machine-learning-using-jupyterhub-and-kubernetes-pydata-delhi-2018)

[[4]. Kubeflow: Cloud-native machine learning with Kubernetes](https://opensource.com/article/18/6/kubeflow)

[[5] Kubeflow docs](https://www.kubeflow.org/docs/)

[[6] Katib : Hyper parameter Tuning](https://github.com/kubeflow/katib)

[[7]. Introducing Seldon Core — Machine Learning Deployment for Kubernetes](https://www.seldon.io/2018/01/26/introducing-seldon-core-machine-learning-deployment-for-kubernetes/)



