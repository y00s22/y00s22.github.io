---
layout: post
title: 자바의 정석/Chapter 6
subtitle: Chapter 6 객체지향 프로그래밍 1
date: 2023-03-15 18:30:00 GMT+0900
tags: [java]
---

# 01 객체지향 언어
## 객체지향언어의 주요 특징
1. 코드 <u>재사용성</u> 이 높다.
2. 코드의 관리가 용이하다. <u>유지보수</u>가 편리하다.
3. 신뢰성이 높은 프로그래밍을 가능하게 한다.
-> **코드의 재사용성이 높고 유지보수가 용이하다**

> 질문🤔
> 
> 보통 절차적 언어랑 객체지향 언어랑 비교를 하는데 그러면 절차적 언어는 안 그렇단 말인가?? 절차적 언어의 대표가 뭐고 진짜 그런지 궁금함

# 02. 클래스와 객체
## 클래스와 객체의 개념
### 정의
- 클래스란?
	- 객체를 정의해 놓은 것
	- 객체의 설계도 또는 틀
	- *객체를 생성*하는데 사용한다.
- 객체란?
	- 실제로 존재하는 것. 사물 또는 개념

> 물론 이건 아주 단순한 설명이고 이걸로는 확실히 이해하기 어렵다. 하도 여러 번 읽고 공부해서 이제는 편하게 받아들이는 개념이지만 처음 들었을 때 무슨 소리인가 했었다.

### 예시
가장 흔한 예시로 붕어빵(객체)과 붕어빵 틀(클래스). 붕어빵 틀을 이용해서 붕어빵을 만든다. 이렇게 같은 틀로 만든 붕어빵은 같은 모양을 하고 있다. 즉, 같은 기능을 한다.

붕어빵과 붕어빵 틀이 다른 것처럼 객체와 클래스도 다른 개념이다. 클래스는 객체를 사용할 때 이용될 뿐 클래스가 객체는 아니다.(클래스 != 객체) 

<u>클래스로부터 객체를 생성하여 사용한다.</u>

오라클 튜토리얼에서는 일종의 blueprint라고 한다. (참고: [What Is a Class? (The Java™ Tutorials)](https://docs.oracle.com/javase/tutorial/java/concepts/class.html)) 

---

## 객체의 구성요소
### 객체란?
> Real-world objects share two characteristics: They all have *state* and *behavior*. (생략) These real-world observations all translate into the world of object-oriented programming.
> 
> 출처:  [What Is an Object? (The Java™ Tutorials)](https://docs.oracle.com/javase/tutorial/java/concepts/object.html)

- 속성
- 기능

으로 나눈다. 즉, <u>객체는 속성과 기능의 집합</u>이다. 그리고 이런 속성과 기능을 그 객체의 **멤버**라고 한다.

### 예시
TV를 예로 든다면, 아래와 같이 생각해 볼 수 있다. 오라클에서는 이렇게 말한다.

> For each object that you see, ask yourself two questions: 
> 
> "What possible states can this object be in?"
> 
> and
> 
> "What possible behavior can this object perform?"

| 객체 구성 요소 | 예시 | |
|:---------------:|:-----------|:-----------|
| 속성 | 크기, 길이, 높이, 색상, 볼륨, 채널 등 | ***-> 멤버변수(variable)*** |
| 기능 | 켜기, 끄기, 불륨 높이기, 볼륨 낮추기, 채널 변경하기 등 | ***-> 메서드(method)*** |

그리고 위와 같이 정리한 내용을 코드로 옮긴다면 아래와 같이 만들수 있다.

~~~ java
class Tv {
	String color;	// 색상
	boolean power;	// 전원 상태
	int channel;	// 채널

	void power() { power = !power; }
	void channelUp() { channel++; }
	void channelDown() { channel--; }
}
~~~

---

## 인스턴스
### 인스턴스란?
클래스로부터 객체를 만드는 과정. 어떤 클래스로부터 만들어진 객체를 그 클래스의 인스턴스(instance)라고 한다. 그렇기 때문에 위에서 말한 것처럼 *객체와 클래스는 다른 개념이다.*

### 객체의 생성과 사용
예를 들어, 위에서 만든 `Tv 클래스`는 그냥 클래스다. 객체나 인스턴스가 아니다. Tv의 청사진이고 실제로 Tv를 생성해야 제품으로된 Tv를 사용할 수 있다.

### 생성 방법
``` java
클래스명 변수명;		// 클래스의 객체를 참조하기 위한 참조변수 선언
변수명 = new 클래스명();	// 클래스의 객체를 생성 후, 객체의 주소를 참조변수에 저장

클래스명 변수명 = new 클래스명(); // 한 번에 생성한 것
```
Tv 클래스의 인스턴스를 생성하여 사용하기 위해서는 아래처럼 하면 된다.
~~~ java
class Store {
	// 인스턴스 생성
	Tv starTv = new Tv();

	// 인스턴스 사용
	starTv.channel = 7;
	starTv.channelDown();
}
~~~

Tv 클래스를 `new`키워드를 이용해서 **인스턴스화** 하였고, 그 결과 `starTv`라는 인스턴스(객체)가 만들어졌다.