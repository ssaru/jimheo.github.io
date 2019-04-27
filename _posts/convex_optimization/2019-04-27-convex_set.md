---
layout: post
title: Convex sets
comments: true
category: Convex Optimization
tags:
- convex
- sets
---



모두의 연구소 풀잎스쿨 7기 Convex Optimization reboot을 진행하고 있다.

학습한 내용을 복습하고 체계적으로 정리하기 위해서 포스팅한다.

<br/>

- 참고자료는 아래와 같다.

  >  [모두를 위한 컨벡스 최적화](https://wikidocs.net/17266)
  > 
  > [Boyd 교수님의 convex optimization ppt](http://web.stanford.edu/class/ee364a/lectures.html)
  > 
  > [Boyd 교수님의 Convex Optimization 2014강의](https://lagunita.stanford.edu/courses/Engineering/CVX101/Winter2014/e70afa892c7f4752974c673f57f68008/)
  > 
  > [Boyd 교수님의 Convex Optimization TextBook](http://web.stanford.edu/~boyd/cvxbook/)
  > 
  > 모두의 연구소 Convex Optimization rebook slack에서 논의된 내용(정현수님)

<br/>

# 들어가기 전에

> 정현수님의 보강자료에서 참고했다.

- 대수학은 수를(AI) 이항(gebr)한다는 의미

  - 여러 계산을 통해 이행하면서 여러 공리들을 만들어나감

  - 즉, 대수학은 계산을 위한 하나의 시스템(체계)

- 계산 시스템을 구성하기 위해 4가지 조건이 필요함

  - 피연산자(Operand)

  - 연산자(Operator)

  - 연산자 우선순위(Precedence, $\times, =, \div, >, +$)

  - 교환법칙, 결합법칙, 등호(Equivalence; *이항과 관련된 개념* $\rightarrow$ $1+2\times3 = 1 + 6 = 7$ )

- Convex set을 설명하기 위해 피연산자(operand)와 연산자(operand)에 대해서만 살펴본다.

<br/>

## 피연산자(operand)

- 피연산자(operand)는, 간단하게 집합(set)을 정의하는 것으로 시작한다.

  - 자연수집합

  - 정수집합

  - 유리수집합

  - 실수집합

- 집합(set)을 정의하는 방법을 알아보자.

  집합(set)은 조건제시법을 통해 정의한다.

  - $S = \{\ x\mid 명제\  \}$

    > - A dog is mammal
    > 
    > - A cat is mammal
    > 
    > $M = \{\ x\ \mid\ x\ \in mammal\ \}$ 와 같이 추상화해서 표현한다.
    > 
    > 이러한 추상화 능력은 인공지능의 기본이 된다.

  - "If x is a horse, then x is mammal" 표현의 경우를 생각해보자.

    - "모든 x가 말이면, 모든 x는 포유류"라는 명제는 "참"이다.

    - $\forall_{x}(H(x) \rightarrow M(x))$

      > $\forall_{x}P(x)$는 모든 $x$에 대해서 $P(x)$가 성립한다는 의미

  - "some horses are pure-blodded"

    - "몇몇 말은 순수혈통이다."

    - $\exists\ {x}(H(x^{P(x)})$

<br/>

## 연산자(operator)

- $A$라는 피연산자(operand)를 연산자(operator)를 통해 얻은 결과 $operator(A)$는 $A$가 속한 집합에 속해야 연산자(operator)라고 할 수 있다.

  - $A\ \in\ S$, $operator(A)\ \in\ S$

  - 집합(set)에 대해서 닫혀있다(closed)라고 표현할 수 있다.

<br/>

## 함수(function)

- 대수학에서 연산자(operator)는 함수(function)의 부분집합개념이다

  - $operator\ \subset \ function$

  - $f\ is\ operator\ if\ f(x) = \sqrt{x},\ x\in\R,\ f(x)\in\R $

  - $f\ is\ function\ if\ f(x)=\sqrt{x},\ x\in Z,\ f(x)\in\R$

    > 즉, 함수는 닫혀있다(closed)라는 제약사항이 없다.

<br/>

## 결론

- 앞으로 배울 convex set은 피연산자(operand)가 된다.

- 그 이후에는 연산자(intersection, affine function, perspective function, linear fractional function)를 배운다.

- convex set과 연산자들(operator)의 의미는 아래와 같다.

  > (convex) operator들을 이용하면 결국 다시 convex set을 만족시킨다.

- 앞으로 배울 내용들에 대한 관계는 아래 도식은 아래와 같다.

  ![relation convex set with operator](https://user-images.githubusercontent.com/13328380/56847180-d4153c00-6912-11e9-9cc3-7144ce8ccd73.png)




