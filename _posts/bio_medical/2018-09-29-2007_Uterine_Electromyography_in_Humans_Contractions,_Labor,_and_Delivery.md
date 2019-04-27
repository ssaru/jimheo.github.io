---
layout: post
title: 논문정리-2007 Uterine Electromyography in Humans - Contractions, Labor, and Delivery 
comments: true
category: BioMedical
tags:
- paper
- ehg
- biomedical
- dsp
- emg
---

![Paper_head]({{site.url}}/images/Uterine_Electromyography_in_Humans_Contractions,_Labor,_and_Delivery/title.png){:.aligncenter}

​    

_주관적인 의견은 이탤릭체로 표기하였습니다._



- [Paper : Uterine Electromyography in Humans - Contractions, Labor, and Delivery](https://link.springer.com/chapter/10.1007/978-3-540-73044-6_32)

​    

### **1. 목적**

**EHG 신호가 현재의 모니터링 시스템을 대체할 수 있는지 검출**

​    

### **2. 동기**

​    

- 현재 전체 산모군에서 20%가 조산군(미국은 10%)

- 현재 진단 시스템은 조산 비율을 줄이지 못함(병원에서의 진찰이 너무 늦기때문?)
- 이를 해결하기 위해서는 조산의 조기 검출이 필요

​    

### **3. 이전연구**

​        

- 이전 방법은 자궁에 전극을 넣어 EHG 획득
- 최근엔 산모 배 위에 전극을 붙여 EHG 신호를 획득
- TOCO를 이용한 예측은 정확하지 않음



- 현재 제일 많이 사용하는 feature extraction method는 RMS 및 주파수 도메인에서의 분석 방법

​    

### **4. 방법론**

​     

_각 방법론에 대해서 자세한 서술은 생략했습니다._

​    

- 323개의 수축/비수축 데이터 수집 ( 조산군/ 비-조산군)
  - 50명의 산모는 2개의 그룹으로 나뉨
    - 24명은 복부 수축
    - 26명은 항문 수축
  - 나머지 44명은 다음과 같은 기준으로 나눔
    - 37주 기준으로 조산군/비-조산군으로 분류
    - 측정 데이터
      - 일(days)
        - 출산 6 / 4 / 2 / 1 일 전 데이터 측정
      - 시간(hours)
        - 출산 48 / 24 / 12 / 8 시간 전 데이터 측정
- 모든 데이터의 측정 시간은 30분
- STM / RMS / PSD / TOCO 신호 분석
  - PSD는 0.34 ~ 1Hz에서 분석
- 평가 방법은 roc 이용

​            

### 5. 결과

​    

- STM / RMS plot은 TOCO Chart와 다를 것이 없음

  - 즉, RMS / STM은 TOCO에 비해 유의미한 의미가 없음

    

- PSD가 유의미한 값 차이가 있다는 것을 보여줌

- 출산 기간이 짧아질 수록 PSD 증가

  - 비-조산군

    - 출산 24시간 전 PSD 증가

  - 조산군

    - 출산 4일 전 PSD 증가

      

![table]({{site.url}}/images/Uterine_Electromyography_in_Humans_Contractions,_Labor,_and_Delivery/table.png){:.aligncenter}



![rms]({{site.url}}/images/Uterine_Electromyography_in_Humans_Contractions,_Labor,_and_Delivery/rms.png){:.aligncenter}



![labor and antepatum]({{site.url}}/images/Uterine_Electromyography_in_Humans_Contractions,_Labor,_and_Delivery/antepatum-labor.png){:.aligncenter}



![pre-term and term]({{site.url}}/images/Uterine_Electromyography_in_Humans_Contractions,_Labor,_and_Delivery/preterm-term.png){:.aligncenter}

​    

### Note

1. 수축 데이터에 대한 언급 없음.
2. 데이터의 sampling rate가 어떻게 되는지 언급 없음.
3. 데이터 취득시, 환자의 age 분포에 대한 언급 없음.
4. 데이터 취득시 사용한 기계 및 스펙언급 없음.