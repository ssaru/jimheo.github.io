---
layout: post
title: 논문정리-Uterine EMG analysis- a dynamic approach for change detection and classification
comments: true
category: BioMedical
tags:
- paper
- ehg
- biomedical
- dsp
- emg
---

![Paper_head]({{site.url}}/images/Uterine_EMG_analysis_a_dynamic_approach_for_change_detection_and_classification/title.png){:.aligncenter}

​    

_주관적인 의견은 이탤릭체로 표기하였습니다._



- [Paper : Uterine EMG analysis- a dynamic approach for change detection and classification](https://ieeexplore.ieee.org/document/844224)

​    

### **1. 목적**

**EHG 신호에서 특정한 사건을 디텍션하고 분류하는 방법을 제안함**

​    

### **2. 동기**

- EHG 신호는 임신여부와 출산을 모니터링하는데 좋은 툴

- EHG 신호를 시간 / 주파수 도메인에서 추출된 feature들은 어떤 특정 사건으로부터 나오는 신호들을 비효율적으로 혹은 효율적으로 분류할 수 있게 함. [6]

- EHG 신호 수집은 비침습에 웨어러블 디바이스 형태로 구현된다면, 빠른 시기(gestational age of 19 weeks)에 측정할 수 있음. [25]

  $\rightarrow$ _임신 후 출산기간은 최대 42주 이상이며, 조산 / 정상 출산을 구분하는 기준은 임신 후 37주가 되기 전에 출산하는 것으로 구분. 따라서 임신 19주 시즘에 EHG 신호를 측정할 수 있다면 이는 2013년 신생아의 조산 분포를 봤을 때, 24주 미만의 분포가 0.04%이므로 합리적으로 보임. 하지만 왜 꼭 19주로 언급했는지는 참조 논문을 확인해봐야할 것으로 판단됨_

  **[reference]** [분만](https://ko.wikipedia.org/wiki/%EB%B6%84%EB%A7%8C) / [조산](https://ko.wikipedia.org/wiki/%EC%A1%B0%EC%82%B0)

​    

### **3. 이전연구**

​    

**3-1. 사전 지식**

- 조산 진찰을 할 때, 보는 신호는 다음과 같음

  - uterine contractions

  - fetus motions

  - Alvarez waves 

    $\rightarrow$ [Electrohysterogram Signal Component Cataloging with Spectral and Time-Frequency Methods](https://run.unl.pt/bitstream/10362/16081/1/Sousa_2015.pdf) 에서 7-1장 참조

  - long-duration low-frequency band (LDBF) waves

- 전통적인 검출 및 분류문제에서 모든 사건들은 시간/주파수 도메인에서 뚜렷하게 확인하기 쉬웠음

- EHG신호에서의 분류는 임신시에는 임신 단계와 사람마다 변하는 특징이 있음

- 임신 단계와 어떤 특정 사람에 대한 고유한 데이터셋이 존재하지 않음

- 따라서 어떤 사전 지식을 이용해 분류하는 것보다 사전지식 없이(unsupervised)방식으로 분류하는게 더 합리적임

  $\rightarrow$ _비지도학습 방법에 대한 합리성 제시_

​       

**3-2. 이전 연구**

- 어떠한 사전지식이 없을 때, 고전적으로 generalized likelihood ratio가 종종 사용됨. [2, 3]
- 어떤 연구자들은 특정한 신호표현에 대해서 generalized likelihood ratio를 사용해서 검출하는 알고리즘을 제안함. [9]
  - 알려지지 않은 신호들을 알려진 주파수 대역과 시간 대역폭을 이용하여 제안해서 검출기의 복잡도를 감소

​    

### **4. 방법론**



**4-1. 가정**

- 변화하는 신호가 수축신호 외의 EMG 신호가 변경되거나 위에서 언급된 조산 진찰시 보는 4가지 신호가 합성되었을 것이라고 가정함
- 이러한 상황에서 특정한 사람 / 특정한 임신 단계의 신호로부터 꾸준하게 확인할 수 있는 feature는 존재 하지 않을 것이라고 가정
- 본 논문에서는 가설 모델에 대한 파라미터에 대해 사전지식이 없는 검출기를 제안함

​           

**4-2. EMG 신호 분석**

EMG신호를 다음과 같이 모델링됨

$$x(t) = EMG(t) + \sum_{i=1}^{n}{S_{i}(t)} + n(t) $$



- $EMG(t)$는 진폭이 가우시안 분포를 갖는 시퀸스 데이터라고 고려될 수 있음. [7, 11]
- $S_{i}(t)$는 짧은 잠재적인 신호나 랜덤하게 나타나는 artifact의 조합물(태아의 움직임이나 기타 움직임)의 중첩
- $n(t)$는 환경이나 기타 상황에서 발생되는 다양한 노이즈. 정적인 가우시안 프로세스라고 가정함

​    

- 여성의 복부로부터 2가지 EMG 신호(slow/fast wave)가 나옴

  - slow wave는 자궁 내압신호

  - fast wave는 추가적으로 2가지 분류가 가능함. [6]

    - 'low'($FW_{L}$  : 0.2 ~ 0.45 Hz)
      - 자궁의 전기적 신호
    - 'fast'($FW_{H}$ : 0.8 ~ 3 Hz)
      - 복부 수축

    $\rightarrow$ 효율적인 복부 수축을 위해 $FW_{L}$에서 $FW_{H}$으로 이동할 때의 $FW_{L}$와 $FW_{H}$의 power ratio를 관찰함. [6]

​    

-  두가지 특성으로 신호 분석이 어려움

  - 자궁 수축시 시간/주파수 도메인에서의 feature가 어떤 상태에서 다른 상태로 변화함. (정상 / 병리학 상태)
  - 태아의 움직임으로부터 생성된 주파수 성분은 다양한 상태 중 하나에서 온 신호
    - 어떤 특정한 상태의 태아의 움직임으로부터 생성된 주파수 성분이 자궁 수축과 유사할 수 있다. [24]

      $\rightarrow$ 이러한 이유로 비지도학습 기반의 검출/분류 알고리즘 개발이 합리적이다. (_비지도학습 방법에 대한 합리성 제시_ )

  

- 고전적인 신호처리 기법은 SNR을 개선시키는 것을 포함한다.

- 노이즈가 정적 특정이 알려져있다면, 이를 이용해 적합한 filter를 사용할 수 있다.

- 최근 많은 연구자들은 WT를 이용해 노이즈를 제거한다.

  - WT를 사용하고, wavelet coefficient thresholding을 이용한 노이즈 제거법이 연구됨. [5, 19]

- 다른 방법으로는 scale level에서 signal decomposition방법을 기반으로 함.

​    

**4-3.  방법**

- 어떤 신호를 주파수 관점에서 multi-scale decomposition을 사용 (WT :: wavelet decomposition)
  - WT는 ECG / EEG / EMG 신호와 같은 생체신호에서 어떤 특징을 확인하거나, 검출시를 설계할 때 사용됨. [14, 20, 28, 29] 

- 비지도학습 기반의 분류는 선택된 scale에서의 variance covariance metric을 계산 및 비교를 하며, 그 이후에는 지도학습 방법인 NN(neural network)를 사용    

  

- 노이즈를 제거하는데는 wavelet decomposition을 사용했으므로,  다차원 검출 방법을 사용하여 탐지를 목적으로 하는 동일한 분해를 사용하는데 관심이 있음

  

  - 순차적 탐지를 모든 scale에 대해 적용
  - scale 상의 신호분해는 bandpass filtering과 동일
  - AR 계수를 필터 자체의 계수(bandpass frequency)로 제한

​    

**4-4. 키워드**

- 알고리즘 flow chart

  ![Flow Chart]({{site.url}}/images/Uterine_EMG_analysis_a_dynamic_approach_for_change_detection_and_classification/flow_chart.png){:.aligncenter}

- Wavelet Decomposition / Dynamic cumulative sum algorithms 



​    

### 5. 결과



- ROC Curve

  ![Flow Chart]({{site.url}}/images/Uterine_EMG_analysis_a_dynamic_approach_for_change_detection_and_classification/ROC_curve.png){:.aligncenter}

   

- Table

  ![Flow Chart]({{site.url}}/images/Uterine_EMG_analysis_a_dynamic_approach_for_change_detection_and_classification/result_table.png){:.aligncenter}



### Note

1. 데이터의 sampling rate가 어떻게 되는지 언급?

2. 데이터 취득시, 환자의 age 분포에 대한 언급?

3. 데이터 취득시 사용한 기계 및 스펙

4. 데이터 분포

   $\rightarrow$ Result and Discussion파트의 D. Result on Real signal에 언급