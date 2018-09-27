---
layout: post
title: 논문정리-ALGORITHM FOR DETECTION OF UTERINE CONTRACTIONS FROM ELECTROHYSTEROGRAM 
comments: true
category: BioMedical
tags:
- paper
- ehg
- biomedical
- dsp
- emg
---

![Paper_head]({{site.url}}/images/Algorithm_for_detection_of_uterine_contractions_from_electrohysterogram/title.png){:.aligncenter}

​    

_주관적인 의견은 이탤릭체로 표기하였습니다._



- [Paper : ALGORITHM FOR DETECTION OF UTERINE CONTRACTIONS FROM ELECTROHYSTEROGRAM ](https://ieeexplore.ieee.org/document/1017198)

​    

### **1. 목적**

**자궁의 전기신호를 분석하고 평가하여 기존의 기계적 방식을 대체할 수 있는지 가능성을 확인**

​    

### **2. 동기**

- 자궁의 기계적 내압상승은 자궁수축으로 일어나는 간접적 현상 
- 기존의 자궁 수축을 탐지하는방법으로 tocography(TOCO)를 사용함
- TOCO를 평가할 때 사용하는 주요 파라미터는 starting time, duration, amplitude, area
- TOCO는 비침습형태로 간단하게 사용할 수 있는 장점이 있음
- 하지만 sensitivity와 accuracy가 낮고, 휴지기의 내압을 측정할 수 없음
- 최근에는 cadiotocography를 통합하여 태아의 상태나 근전도 신호를 측정함

​    

### **3. 이전연구**

​    

- 1960년도부터 EMG신호에 대한 관심이 많아지면서, 산모의 배에서 EHG를 수집하여 분석하려는 시도가 있었음. [1]

  $\rightarrow$ 초기연구에서는 EHG 신호에 대한 특성에 대한 이해가 부족해서 연구 및 개발에 어려움이 있었음

- 이후 Digital Signal Processing이 발전되고, EHG 신호에 대한 이해가 쌓이면서 time/frequency parameter를 결정할 수 있게 됨. [2, 3]

​    

**3-1. 사전 지식**

- EHG 신호는 2가지 요소로 구성됨.

  - Fast wave
  - Slow wave
    - contraction wave와 관련이 있음

​    

### **4. 방법론**

​     

_각 방법론에 대해서 자세한 서술은 생략했습니다._

​    

- Filteration method

  - 어떤 복잡한 신호는 여러 신호의 합성신호라고 가정하고, 이를 분해해서 사용하는 방식

  - 분해방법은 non-linear 연산과 low-pass filter로 구성됨

  - non-linear연산 후에 0.02Hz low-pass filter를 사용

    $\rightarrow$ _non-linear 연산이 어떤 연산인지에 대한 내용은 논문에 없으나, WT나 WPD로 추측됨_

  - EHG 신호에 대한 다른 모델링은 [4] 논문에 기반이 됨

    - _기계적 활동의 위상이 어떠한 전위활동의 가능성의 주파수와 관련이 있음?_ 
    - 하지만 주파수 변조는 신호 차동으로 인한 진폭 변조로 변경될 수 있음
    - 따라서 변조 신호를 결정하기 위해 진폭 복조를 수행함

​    

- Statistical method
  - RMS와 Hanning window를 이용한 envelop을 구함

​        

### 1-5. 결과

​    

- 112개의 자궁 수축 신호를 사용함
- 평균 자궁수축 신호의 duration은 40 $\pm$ 5 min

​    

- $EHG-F$는 filtration method를 사용한 결과
- $EHG-S$는 statistical method를 사용한 결과
- $EHG-F^{*}$는 진폭 복조를 이용한 결과



- $EHG-F^{*}$와 $EHG-F$신호를 보면, 주파수 변조의 영향이 작고, 진폭 변조된 신호가 EHG 신호에 좋은 모델인 것을 확인할 수 있음

​    

- 자궁 수축 탐지 결과 논문에서 제안한 EHG신호 기반의 2가지 방법을 사용했을 경우 87%, TOCO를 사용했을 경우 96%의 정확도를 보였음

​    

![result]({{site.url}}/images/Algorithm_for_detection_of_uterine_contractions_from_electrohysterogram/result.png){:.aligncenter}



### Note

1. 데이터의 sampling rate가 어떻게 되는지 언급?

2. 데이터 취득시, 환자의 age 분포에 대한 언급?

3. 데이터 취득시 사용한 기계 및 스펙

4. 데이터 분포

   $\rightarrow$ Result and Discussion파트의 D. Result on Real signal에 언급

5. 방법론의 설명 부실