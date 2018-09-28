---
layout: post
title: 논문정리-An unsupervised classification method of uterine electromyography signals using wavelet decomposition 
comments: true
category: BioMedical
tags:
- paper
- ehg
- biomedical
- dsp
- emg
- wavelet decomposition
---

![Paper_head]({{site.url}}/images/An_unsupervised_classification_method_of_uterine_electromyography_signals_using_wavelet_decomposition/title.png){:.aligncenter}

​    

_주관적인 의견은 이탤릭체로 표기하였습니다._



- [Paper : An unsupervised classification method of uterine electromyography signals using wavelet decomposition](https://ieeexplore.ieee.org/document/1403124)

​    

### **1. 목적**

**EMG 신호를 이용해 자궁수축을 분류**

​    

### **2. 동기**

​    

- 조산은 중요한 문제로 남아있음. 

- 기존 방법은 tocolytic treatments(TOCO)를 사용하는 것
- 이동하면서 측정이 가능한 EHG기반의 조산예측 감지기가 필요하게 됨. [3]

​    

### **3. 이전연구**

​        

- EHG신호는 출산 및 임신 모니터링에 적용가능하다는 것이 증명됨
- 전통적으로 EHG신호를 분석하는데 사용하는 방법은 frequency 성분을 보는 것
- EHG신호는 다른 신호랑 섞여서 들어옴
- 또한 임신 기간에 따라서 신호가 변함
- 따라서 이를 분리해서 분석해야함



- 주파수 특징을 기반으로한 전통적인 feature는 실제 사전 클래스를 알려주는 것이 불가능.
- 부분적으로 그러한 것을 찾는다고 할지라고, 보편적인 특정이라고 이야기하기엔 현실적이지 않음
- 따라서 Classification도 class가 여러개로 확대될 여지가 있으니, unsupervised learning을 하는게 합리적임

- wavelet transform(WT)은 EHG신호를 설명해주는 좋은 주파수 툴. [4]
- 본 논문에서는 이러한 WT를 parameter extraction tool(feature extractor)로 사용

​    

### **4. 방법론**

​     

_각 방법론에 대해서 자세한 서술은 생략했습니다._

​    

- Wavelet transform과 K-means와 비슷한 형태의 unsupervised classification을 사용함



1. Wavelet Transform
   - wavelet choice
     - symlet wavelet 사용
   - scales choice
     - multidimensional representation based on the wavelet decomposition [6] 사용



2. Parameter extraction
   - 각 신호에 대한 variance 계산



3. unsupervised classification

   _centroid distance를 Fisher test [8]로하는 K-means를 사용한 듯_

   _threshold값을 높게 주고 낮게 주냐에 따라서 클래스의 수가 늘고 준다는데, 이는 cluster의 갯수인듯_

​        

### 5. 결과

​    

![result]({{site.url}}/images/An_unsupervised_classification_method_of_uterine_electromyography_signals_using_wavelet_decomposition/result.png){:.aligncenter}

​    

### Note

1. 수축 데이터에 대한 언급 없음.
2. 데이터의 sampling rate가 어떻게 되는지 언급 없음.
3. 데이터 취득시, 환자의 age 분포에 대한 언급 없음.
4. 데이터 취득시 사용한 기계 및 스펙언급 없음.