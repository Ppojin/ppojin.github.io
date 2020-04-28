---
layout: post
title:  "AOP"
date:   2020-04-22 16:10:37 +0900
categories: java
---

# AOP
- 관점지향 프로그래밍
- 높은 응집도
- 보안인증 혹은 로깅과 같은 공통 기능들을 모듈로써 따로 반들어두고, 비즈니스 로직에서 이용하여 사용하는 개발 방법.

### Target
- 핵심 기능
- 비즈니스 로직
- `Advice`을 부여할 대상

  #### JoinPoint
  - `Advice`가 적용될 수 있는 위치
  - `Advice` 와 `Target` 이 합쳐지는 지점
  - `Target` 의 `Method`
  - `Target`객체가 구현한 Interface의 모든 Method는 `JoinPoint`가 된다

### Advice
- 부가기능
- 공통기능
- `Target`에 재공할 부가기능이 담긴 모듈

  #### 동작시점
  - before: 메소드 실행 전 동작
  - After: 메소드 실행 후에 동작
  - After-returning: 메소드 정상 작동 후에 동작
  - After-throwing: 에러 발생 후 동작
  - Around: 메소드 호출 이전, 이후, 예외발생 등 모든 시전에서 동작

  #### PointCut
  - `Advice`를 적용할 타겟의 Method를 선별하는정규 표현식
  - `PointCut`표현식은 excution으로 시작하고 메서드의 Signature를 비교하는 방법을 주로 이용함 
  - `Advice`를 어디에 적용하는지 결정하는

### Aspect 
- `Advice` + `PointCut`
- `AOP`의 기본 모듈
- Singleton 형태의 객체로 존재함
- `AOP` 개념을 적용하면 `Target`사이에 침투된 `Advice`를 독립적인 `Aspact`로 구분해 낼 수 있다
- 구분된 부가기능 `Aspact`를 런타임 중 필요한 위치에 동적으로 참여하게 할 수 있다

  #### Advisor
  - `Advice` + `PointCut`
  - `Aspect`
  - Spring AOP 에서만 사용되는 용어

### Weaving
- `PointCut`에 의해서 결정된 타겟의 `JoinPoint`에 `Advice`을 삽입하는 과정
- AOP가 `Target`의 코드에 영향을 주지 않으면서 `Advice`를 추가할 수 있도록 해주는 핵심적인 처리과정

---

## 포인트컷 표현식
포인트컷을 이용하면 어드바이스 메소드가 적용될 비즈니스 메소드를 정확하게 필터링할 수 있다.

### 지시자(PCD, AspectJ pointcut designators)의 종류
몇가지들이 있다. 아래에선 execution 을 사용한 표현식에 대해 알아본다.

- `execution` : 가장 정교한 포인트컷을 만들수 있다. 리턴타입 패키지경로 클래스명 메소드명(매개변수)
- `within` : 타입패턴 내에 해당하는 모든 것들을 포인트컷
- `bean` : bean이름으로 포인트컷

### 리턴타입 지정
- `*`:	모든 리턴타입 허용
- `void`:	리턴타입이 void인 메소드 선택
- `!void`:	리턴타입이 void가 아닌 메소드 선택

### 패키지 지정
- `com.devljh.domain`: 정확하게 com.devljh.domain 패키지만 선택
- `com.devljh.domain..`: com.devljh.domain 패키지로 시작하는 모든 패키지 선택

### 클래스 지정
- `UserBO`: 정확하게 UserBO 클래스만 선택
- `*BO`: 이름이 BO로 끝나는 클래스만 선택
- `BaseObject+`:	클래스 이름 뒤에 '+'가 붙으면 해당 클래스로부터 파생된 모든 자식 클래스 선택, 인터페이스 이름 뒤에 '+'가 붙으면 해당 인터페이스를 구현한 모든 클래스 선택

### 메소드 지정
- `*(..)`:	모든 메소드 선택
- `update*(..)`:	메소드명이 update로 시작하는 모든 메소드 선택

### 매개변수 지정
- `(..)`: 모든 매개변수
- `(*)`:	반드시 1개의 매개변수를 가지는 메소드만 선택
- `(com.devljh.domain.user.model.User)`: 매개변수로 User를 가지는 메소드만 선택. 꼭 풀패키지명이 있어야함
- `(!com.devljh.domain.user.model.User)`: 매개변수로 User를 가지지않는 메소드만 선택
- `(Integer, ..)`:	한 개 이상의 매개변수를 가지되, 첫 번째 매개변수의 타입이 Integer인 메소드만 선택
- `(Integer, *)`:	반드시 두 개의 매개변수를 가지되, 첫 번째 매개변수의 타입이 Integer인 메소드만 선택

---

## JoinPoint 인터페이스
어드바이스 메소드를 의미있게 구현하려면 클라이언트가 호출한 비즈니스 메소드의 정보가 필요하다. 예를들면 예외가 터졌는데, 예외발생한 메소드의 이름이 뭔지 등을 기록할 필요가 있을 수 있다. 이럴때 JoinPoint 인터페이스가 제공하는 유용한 API들이 있다.

### 메소드
- `Signature getSignature()`:	클라이언트가 호출한 메소드의 시그니처(리턴타입, 이름, 매개변수) 정보가 저장된 Signature 객체 리턴
- `Object getTarget()`:	클라이언트가 호출한 비즈니스 메소드를 포함하는 비즈니스 객체 리턴
- `Object[] getArgs()`:	클라이언트가 메소드를 호출할 때 넘겨준 인자 목록을 Object 배열 로 리턴
### Signature API 메소드
- `String getName()`:	클라이언트가 호출한 메소드 이름 리턴
- `String toLongString()`:	클라이언트가 호출한 메소드의 리턴타입, 이름, 매개변수(시그니처)를 패키지 경로까지 포함 하여 리턴
- `String toShortString()`:	클라이언트가 호출한 메소드 시그니처를 축약한 문자열로 리턴

### 사용법
JoinPoint를 어드바이스 메소드 매개변수로 선언해야한다. 이때 인자는 스프링 컨테이너가 넘겨준다. ex. 메소드명(JoinPoint jp)

이때 Around 어드바이스만 다른 어드바이스와 약간 다른데, ProceedingJoinPoint 객체를 인자로 선언해야한다.(proceed() 등이 추가로 구현되어있음) ProceedingJoinPoint 는 JoinPoint를 상속받는다


출처: [빨간색코딩](https://sjh836.tistory.com/157 )
