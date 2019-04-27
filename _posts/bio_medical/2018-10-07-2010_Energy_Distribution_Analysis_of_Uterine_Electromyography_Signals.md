---
layout: post
title: 논문정리-2010 Energy Distribution Analysis of Uterine_Electromyography_Signals 
comments: true
category: BioMedical
tags:
- paper
- ehg
- biomedical
- dsp
- emg
---

![Paper_head]({{site.url}}/images/2010_Energy_Distribution_Analysis_of_Uterine_Electromyography_Signals/title.png){:.aligncenter}

​    

_주관적인 의견은 이탤릭체로 표기하였습니다._



- [Paper : Energy Distribution Analysis of Uterine_Electromyography_Signals](http://www.jmbe.org.tw/files/545/public/545-2222-1-PB.pdf)

​    

### **1. 목적**

**임신/노동시의 자궁 EHG 신호의 energy distribution에 대해서 연구함**

​    

### **2. 이전연구**

​        

- 임신부의 출산 , EHG의 spectral이 바뀜. [12]
- 자궁 수축에 대한 특징은 저주파에서 발견된다는 것으로 확인. [12]
- 활동시, 고주파에서 전기적 활동의 에너지가 증가하는 것을 확인할 수 있음. [13]
- wavelet transform(WT)를 이용하여 EHG의 주파수 성분에 대해서 확인. [14]
  - 주파수 성분이 임산부 마다, 임신 시간에 따라서 변함

​    

### **3. 방법론**

​     

_각 방법론에 대해서 자세한 서술은 생략했습니다._

​    

**Wavelet packet transform(WPT)를 이용하여 신호 에너지를 각기 다른 주파수 대역에서 분리하는 방법을 사용**

​    

- 11명의 여성에 대한 데이터를 사용
  - 5명은 임신시 데이터 측정 ( 33 ~ 39주의 임신기간)
  - 6명은 노동시 데이터 측정 
    - 5명은 임신한 여성( 39 ~ 42주의 임신시간)
    - 2명은 다른 임신기간에 데이터 측정
  - toco 데이터를 이용한 자궁수축 labeling
  - 총 30개의 자궁 수축 데이터와 30개의 노동으로 인한 수축데이터 수집
  - 측정 기기는 Landspitali University Hospital에서 만든 장비 사용
  - 샘플링 주파수는 200Hz
    - anti-aliasing filter를 가지고 있으며, 100Hz에서 주파수 cut-off
  - 수축 데이터는 모두 standard deviation기반의 normalization 진행. [14]

​    

![sensor position]({{site.url}}/images/2010_Energy_Distribution_Analysis_of_Uterine_Electromyography_Signals/sensor_position.png){:.aligncenter}            

​    

### 4. 결과

​    

- 에너지 분포가 임신기간에 따라서 변화함
  - 임신한 기간이 길어질수록 저주파의 에너지 분포는 감소
  - 임신한 기간이 길어질수록 고주파의 에너지 분포는 증가

​    

- 수축 데이터 에너지의 평균을 측정한 결과
  - 저주파에서의 임신시 에너지 평균 > 노동시 에너지 평균
  - 고주파에서의 임신시 에너지 평균 < 노동시 에너지 평균

​    

![sensor position]({{site.url}}/images/2010_Energy_Distribution_Analysis_of_Uterine_Electromyography_Signals/result-1.png){:.aligncenter}



![sensor position]({{site.url}}/images/2010_Energy_Distribution_Analysis_of_Uterine_Electromyography_Signals/result-2.png){:.aligncenter}



![sensor position]({{site.url}}/images/2010_Energy_Distribution_Analysis_of_Uterine_Electromyography_Signals/result-3.png){:.aligncenter}

