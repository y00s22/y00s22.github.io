---
layout: post
title: Java 8 - default methods
subtitle: interface에서 사용하는 default method에 대해서
date: 2023-04-06 19:13:00 GMT+0900
tags: [java]
---

책을 보면서 공부하는데 `default` 라는 키워드가 나왔는데 의미를 모르겠어서 찾아보게 되었다. 일단 [오라클 공식 문서](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) 찾아봤다.

# 인터페이스 정의
공식 문서에 따르면 인터페이스에는 다음과 같은 요소만 사용할 수 있다.
> The interface body can contain [abstract methods](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html), [default methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html), and [static methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html#static).

각 메서드들은 명시만 되어 있고 실제 구현은 이 인터페이스를 사용하는 구현체에서 구현한다. 

일반적으로 아무 제어자가 붙어있지 않으면 `abstract methods`이다. 반환타입, 메소드 명, 파라미터만 선언하고 method body는 없다.
``` java
public interface TestInterface {
	String getMethodName(); 
	// abstract String getMethodName(); 와 동일하다.
}
```


# default method란?
## 사용 방법
java 8에서 새로 생긴 기능이다.

메서드에 `default`키워드를 붙여서 method body를 작성할 수 있게 해준다. 인터페이스를 선언하면서 로직도 같이 구현 할 수 있게 되었다. 반드시 method body가 있어야 하며 method body가 없으면 아래와 같이 컴파일 에러가 발생한다.

![default method error example code](/assets/images/interface-default-methods-01.png)

코드 상에서 다음과 같이 사용한다. 물론 구현체에서 오버라이드 할 수 있다.
``` java
public interface TestInterface {
    default String getMethodName(){
        System.out.println("TestInterface.getMethodName");
    }
}
```

## 왜 생겼을까? 언제 사용할까?
결론부터 말하면 **인터페이스에 새로운 메서드를 추가할 때 기존에 그 인터페이스의 구현체에서 에러가 발생하지 않도록 하기 위해서** 생겼다. 

인터페이스에서 모든 메서드는 `public`으로 선언해야 하면 구현체에서는 모든 메서드를 구현해야 한다. 그런데 A라는 인터페이스에 새로운 메서드를 추가하면 모든 구현체에서 이 추가된 메서드를 구현해야 한다. 그렇지 않으면 에러가 발생한다.

이때 이 에러를 방지하기 위해서 `default method`가 나왔다.

위 예제에서 보다시피 default method는 구현체가 아니라 인터페이스에서 메서드를 구현한다. 그래서 기존 구현체에서 별도로 구현할 필요가 없으므로 컴파일시에 에러가 발생하지 않는다.

## 예제
예를 들면 다음과 같은 `TimeClient` 인터페이스와 abstract methods가 있고,
``` java
import java.time.*; 
 
public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second);
    LocalDateTime getLocalDateTime();
}
```

이를 구현하는 구현체인 `SimpleTimeClient` 가 있다고 하자.
``` java
public class SimpleTimeClient implements TimeClient {
    
    private LocalDateTime dateAndTime;

    // 내용 생략

    public static void main(String... args) {
        TimeClient myTimeClient = new SimpleTimeClient();
        System.out.println(myTimeClient.toString());
    }
}
```

`TimeClient` 인터페이스에 새로운 기능을 추가한다고 가정해보자.
``` java
public interface TimeClient {
	// 기존 내용 생략
	// 새로 추가된 메서드                  
	ZonedDateTime getZonedDateTime(String zoneString);
}
```

그러면 구현체인 `SimpleTimeClient`에서도 수정이 일어나고, `getZonedDateTime` 메서드를 구현해야 한다. 그렇지 않으면 아래와 같이 에러가 발생한다. 만약 `TimeClient` 인터페이스의 구현체가 하나가 아니라 n개라면 이런 수정과 구현이 n번 일어나게 된다.
![interface-default-methods-02](/assets/images/interface-default-methods-02.png)

그러나 `default`를 사용하면 이런 에러를 방지할 수 있다.
``` java
public interface TimeClient {
    // 기존 내용 생략
    // 새로 추가된 메서드
    static ZoneId getZoneId (String zoneString) {
        try {
            return ZoneId.of(zoneString);
        } catch (DateTimeException e) {
            System.err.println("Invalid time zone: " + zoneString + "; using default time zone instead.");
            return ZoneId.systemDefault();
        }
    }

    default ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZoneId(zoneString));
    }
}
```

## 문제점
자바에서 클래스의 다중 상속은 불가능하다. 그렇지만 인터페이스는 다중 상속이 가능하다.
그런데 만약 구현체가 하나 이상의 인터페이스를 구현한다면 어떻게 될까? 이때 동일한 이름의 default 메서드가 1개 이상이라면 어떻게 될까?


예를 들어 아래와 같은 인터페이스를 구현하려고 하면 어떻게 될까?
``` java
public interface Interface01 {
    default void defaultMethod() {
        System.out.println("Interface01.defaultMethod");
    }
}

public interface Interface02 {
    default void defaultMethod() {
        System.out.println("Interface02.defaultMethod");
    }
}
```

이때 구현체는 다음과 같은 에러가 발생하게 된다. 어떤 인터페이스의 메서드를 호출해야 할지 모르기 때문에 다중 상속으로 인한 컴파일 에러가 발생한다.
![interface-default-methods-03](/assets/images/interface-default-methods-03.png)

이를 해결하기 위해서는 구현체에서 `defaultMethod()`를 override 해주어야 한다.
``` java
public class InterfaceImplements 
        implements Interface01, Interface02 {
    @Override
    public void defaultMethod() {
        System.out.println("InterfaceImplements.defaultMethod");
    }
}
```

이처럼 자바에서 볼 수 없었던 다중상속 문제가 발생할 수 있으니 주의해서 사용해야 한다.