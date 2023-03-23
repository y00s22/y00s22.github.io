---
layout: post
title: 스프링 입문을 위한 자바 객체 지향의 원리와 이해 2장
subtitle: 02 자바와 절차적/구조적 프로그래밍
date: 2023-03-24 00:12:58 GMT+0900
tags: [oop]
---
![summary_mindmap](/assets/images/java-oop-for-spring-ch2.jpg)


# 자바 프로그램의 개발과 구동
## Java 개발 및 실행 환경
- 자바가 만들어 주는 가상 세계
	* 소프트웨어 개발 도구 -> JDK 자바 개발 도구
	* 운영체제 -> JRE 자바 실행 환경
	* 하드웨어(물리적 컴퓨터) -> JVM 자바 가상 기계
* <u>자바의 가상 세계는 이러한 현실 세계를 그대로 모방하여 자바 프로그램을 개발, 구동한다.</u>


## 구성
1. JDK
	- 자바 개발 도구(Java Development Kit)
	- 자바 소스 코드를 컴파일하고 실행할 수 있다.
	- 자바 소스 컴파일러 포함
		- 자바 소스파일에서 자바 목적 파일 생성
		- javac를 비롯한 다양한 개발도구 포함
2. JRE
	- 자바 실행 환경(Java Runtime Environment)
	- 자바 프로그램을 실행하기 위한 런타임 환경 제공
	- 자바 프로그램 실행기 포함
		- 각 OS 별로 다르다
	- JVM과 자바 클래스 라이브러리 등을 포함
3. JVM
	- 자바 가상 기계(Java Vitual Machine)
	- 자바 바이트코드를 실제로 실행
	- 개발자가 본인이 사용중인 플랫폼에 맞는 JDK를 설치하여 개발하면 JVM이 운영 체제와 상관없이 동일한 동작을 보장한다.
		- 플랫폼에 독립적으로 실행 가능하다.


---


# 자바에 존재하는 절차적/구조적 프로그래밍의 유산
1. 절차적 프로그래밍
	- goto를 쓰지 말라 -> why?
		- 프로그램의 실행 순서가 너무 복잡해질수 있다.

2. 구조적 프로그래밍
	- 함수를 써라 -> why?
		- 중복 코드를 한 곳에 모아서 관리할 수 있다.
		- 논리를 함수 단위로 분리하여 이해하기 쉬운 코드 작성 가능
	- 전역 변수보다는 지역 변수를 써라

3. 자바에 남은 부분
	- goto문 
		- 제어 흐름
		- 순서도와 제어문
	- 함수
		- 메서드로 표현
		- Function과 Method는 동일하다


---


# 다시 보는 main() 메서드: 메서드 스택 프레임
## 프로그램이 메모리를 사용하는 방식
- 공통된 메모리 사용 방식
<table style="border: 1px solid;">
	<colgroup>
		<col width="50%" />
		<col width="50%" />
	</colgroup>
	<tbody style="text-align:center;background: #f3f3f3;height: 70px;">
		<tr>
			<td markdown="span">코드 실행 영역</td>
			<td markdown="span">데이터 저장 영역</td>
		</tr>
	</tbody>
</table>

- 객체 지향 프로그램의 메모리 사용 방식
	- 데이터 저장영역을 다음과 같이 분할한다.
	- 이 분할된 부분을 **이하 T 메모리 구조**라고 부른다.
<table style="text-align:center; height: 140px; border: 1px solid;">
  <tr>
    <td rowspan="2" style="color: #999797">코드 실행 영역</td>
    <td colspan="2" style="background: #f3f3f3;">Static 영역<br>(클래스들의 놀이터)</td>
  </tr>
  <tr style="background: #f3f3f3;">
    <td>Stack 영역<br>(메서드들의 놀이터)</td>
    <td>Heap 영역<br>(객체들의 놀이터)</td>
  </tr>
</table>


## T 메모리 구조의 변화
``` java
public class Start {
	public static void main(String[] args) {
		System.out.println("Hello OPP!!!");
	}
}
```

위 코드를 실행시킬 때의 T 메모리 구조의 변화는 다음과 같다.


### 1. 전처리 과정
1. [JRE] main() 메서드가 있는지 확인
2. [JVM] 부팅
3. [JVM] 목적 파일 실행
4. [JVM] java.lang 같은 패키지를 Static 영역에 가져다 놓는다.
5. [JVM] 개발자가 작성한 모든 클래스와 패키지를 Static 영역에 가져다 놓는다.

-> 전처리 과정 완료

