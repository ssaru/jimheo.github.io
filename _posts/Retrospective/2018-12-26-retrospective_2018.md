---
layout: post
title: 2018회고-원하는 삶을 만들기 위한 준비과정
comments: true
category: Retrospective
tags:
- Retrospective
- 2018

---

2018년 8월 `딥러닝을 공부하는 청년백수 모임`과 함께하고 난 이후부터 오랫동안 잊고있었던 나를 되돌아보고 끊임없이 성장하기 위한 플랜을 세우게 되었다. 

스스로를 체계적으로 곱씹으면서 행동하던 기간은 2018년 하반기정도이나, 이를 되돌아보며 앞으로 올 2019년을 준비해보고자 한다.

​    

## 목차

- 발단(왜 나는 삶을 체계적으로 준비하게 되었는가?)
- 딥러닝을 공부하는 청년백수 모임
- 활동
  - Self Driving Lab
  - Object Detection 구현모임
- 2018년 잘한 점
- 2018년 아쉬웠던 점
- 2019년을 준비하며
- 정리 및 맺음말

​    

## 발단

지금 2018년 회고록을 쓰게끔 나를 이끌어준 중대한 사건이 있다. 그런 사건이 있었다는 것만 언급하고 상세한 이야기는 생략하도록 하겠다. 아마 그 때 그 사건이 없었더라면, 이 회고록도 없었을 것이라고 장담한다. 

- 내 할 일에 집중하지 못하고, 다른 것에 집중하여 팀원에게 폐를 끼친 것
- 그것에 대한 팀원의 솔직한 피드백
- 내가 살아가지던데로 살아왔구나라는 깨달음



이렇게 3단계로 사고의 흐름을 거치면서, 문득 군 제대후에 편입학을 위해서 졸업플랜을 일/월/년별 목표로 나누어서 이를 실행하여 이루어냈던 것이 떠올랐고, 페이스 북에서 `딥러닝을 공부하는 청년백수 모임`의 백수콘 자료를 보면서 `꼭 이걸 해야겠다라는 결심이 들었다.`



![retrospective](https://user-images.githubusercontent.com/13328380/50444212-9d222000-094a-11e9-9477-7ca440885b98.png)

​    

## 딥러닝을 공부하는 청년백수 모임



![manager](https://user-images.githubusercontent.com/13328380/50444506-26862200-094c-11e9-8473-028ac834697e.png)



백수콘 자료를 보고, `딥러닝을 공부하는 청년백수 모임`과 함께하고싶다는 생각이 들고 나서 뒤돌아보니 어느센가 `딥러닝을 공부하는 청년백수 모임`의 멤버가 되어있었고, (서포터 역활을 자처하지만) 운영자님의 섬세한 배려로 공동운영자가 되어있었다. 2018년엔 다음과 같은 행사들을 함께 하게되었다.  

- `양재 피크닉`
- `강화도 MT`
-  `스타트업 견학`



![picnic](https://user-images.githubusercontent.com/13328380/50444498-1d955080-094c-11e9-9cc9-392c3662b8e0.png)



![mt](https://user-images.githubusercontent.com/13328380/50444496-1cfcba00-094c-11e9-81f7-0680d4bc5488.png)



![startup](https://user-images.githubusercontent.com/13328380/50444497-1d955080-094c-11e9-81ae-ed9c74e29b44.png)



`딥러닝을 공부하는 청년백수 모임`활동을 진행하면서 성장을 추구할 때 자발적인 고독을 선택할 수 밖에 없는 시기가 오는데, Slack 채널을  통해서 서로의 안부를 묻고 격려하며 때로는 만나서 서로를 응원하는 모습에서 굉장히 큰 위안과 위로를 받고있는 느낌을 많이 받았다.

​    

## 활동



커뮤니티 성격이 아닌, 프로젝트성격의 활동으로 2018년에 진행한 활동은 다음과 같은 활동이 있다.

- Self-Driving Lab [2017.08 ~ 2018.08]
- Object Detection Implementation Project [2018.08 ~ 진행 중]

​    

### Self-Driving Lab [2017.08~2018.08]  

Self-Driving Lab은 Deep Advanced Lab의 랩장님이 이직으로 인해 대구로 거주지를 옮기게 되면서 Self-Driving Lab으로 변경되어 자율차량에 관련된 연구를 하게되었다.  Self-Driving Lab에서 주로 진행했던 것들은 다음과 같다.

​    

#### Paper Review

- Overfeat [v]
- R-CNN [v]
- Fast R-CNN [v]
- Faster R-CNN [v]
- Mask R-CNN [v]
- YOLO [v]
- YOLO 9000 [v]
- YOLO v3 [v]
- MegDet [v]
- SSD
- RRC
- Forcal Loss for Dense Object Detection
- FPN
- GoogleNet
- DeepLabv3
- Monodepth [v]
- End to End Learning for Self-Driving Cars
- End-to-end Learning of Driving Models from Large-scale Video Datasets [v]
- BinaryConnect [v]
- Binarized Neural Networks [v]
- XNOR Net [v]



내가 발표를 맡았던 부분은 [v] 마크로 표기했다.

​    

#### ROB Challenge

읽었던 monodepth 논문을 baseline으로 하여 ROB challenge의 Single Depth Prediction 파트로 시도를 했었고, Indoor/outdoor를 요구하는 ROB challenge에는 제출을하지 못했지만 KITTI의 benchmark에서 15위를 기록하며 마무리를 지었다.

​    

### Object Detection Implementation Project [2018.08 ~ 진행 중]

Object Detection 구현 모임은 Object Detection 논문은 많이 읽었는데 실제로 Object Detection에 대해서 정확하게 이해하고 있고, 이를 구현할 수 있는지에 대한 회의가 들어 진행하고 있는 프로젝트이다.



현재는 YOLO에 대해서 구현이 80% 진행되었고, Pytorch Tutorial Competition 2018에 참가하여 tutorial 문서 / 코드를 제출했다.

​    

## 2018년 좋았던 점

- 딥러닝을 공부하는 청년백수 모임을 참가한 것
  - 여러 도메인에서 업무를 하고 있는 사람들을 만나게 되었다.
  - 그들이 어떤 방식으로 삶을 대하고있는지에 대한 것을 일부 엿볼 수 있었다.
  - 내 일상생활을 Agile 방식으로 운영하는 것을 통해서 내 의식이나 삶을 흘러가는데로 두지 않고, 이를 잡아서 내가 원하는 방향으로 움직일 수 있는 힘을 주었다.
  - 커뮤니티 분들께 얻는 힐링
- TIL 시작 및 Git blog 시작
- Daily Report 작성

​    

## 2018년 아쉬웠던 점

- 건강에 소홀한 것
- 너무 많은 일을 동시에 한 것
  - 같은 시간에 여러가지 일을 동시에 할 수 없음을 깨달음
  - 학습은 종류가 메타지식일 수록 단시간에 채득되지 않으며 결과 또한 단시간에 나타나지 않음
- 수면관리에 소홀한 것
- 재태크에 소홀한 것
- Self Driving Lab 활동 이후에 논문 읽기 활동 소홀한 것
- 11월 이후, 각 task실행시간이 많이 루즈해진 점
- 책을 많이 읽지 못한 점

​    

## 2019년을 준비하며

이제 곧 2019년을 맞이하게 된다. 2019년에는 대학원과 회사업무의 병행 / 2020년의 지출목표 / 등으로 인해서 삶이 더 타이트하게 돌아갈 예정이다. 

2019년을 위해서 Product BackLog Item을 정해보았다.

- [ 중요도 : 90 / 식단] 삼시세끼 잘 먹어보기
- [ 중요도 : 90 / 수면] 규칙적 수면습관 기르기
- [ 중요도 : 90 / 건강] 근지구력, 근력 기르기
- [ 중요도 : 80 / finance] 지출 설계, 예측 및 통제하기
- [ 중요도 : 80 / Analytics] 기록 분석을 위한 자동화
- [ 중요도 : 30 / Paper] 1주일 2개 paper 리딩 후 TIL
- [ 중요도 : 30 / 영어] 공인인증 영어성적 획득하기
- [ 중요도 : 30 / 독서] 한달에 한권씩 책읽고 TIL
- [ 중요도 : 30 / 기술 Stack 계발] 공부 후 TIL
- [ 중요도 : 30 / 대학원 진학 관련 사이드 프로젝트 미리 진행] 경량화 네트워크
- [ 중요도 : 10 / 디텍션 구현모임] Object Detection 구현



현재는 대략적인 Product BackLog Item만 정한 상태이나, 새해가 오기전에 Sprint Backlog를 위한 상세 설계 및 추청을 진행할 예정이며, 각 Product BackLog Item들은 상/하반기로 나누어서 중간 회고를 진행할 예정이다.

​    

## 정리 및 맺음말

회고를 하고보니 삶을 정리하고 원하는 방향으로 틀어보기 시작한 기간이 4개월밖에 안됬음에도 불구하고, 8개월이 지난거 같은 느낌을 받았다. 그렇게 느낀만큼 짧은 시간이 나에게는 압축성장을 할 수 있는 시간이었으면 기대한다. 모든 것을 이곳에 기록하진 못하였지만 2018년은 나에게 참 다사다난했던 해였다. 이제 오는 2019년은 조금 더 스스로를 잘 갈무리하고 원하는 것을 많이 달성할 수 있는 한해가 되었으면 좋겠다.

