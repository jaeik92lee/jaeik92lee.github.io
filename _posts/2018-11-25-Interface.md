---
layout: post
title: "Interface (작성중)"
author: "Jaeik Lee"
categories: [java]
tags: [interface, standard, independent, java, strategy, pattern, design patter]
---

# Interface

---

## **Interface 란?**
```
# interface는 일종의 추상클래스 ( 여러 클래스가 공통의 기능을 가질 수 있도록 제약 해 놓은 것 )
```

---

## **interface 사용 이유(예제)**
```
1. 개발시간을 단축시킬 수 있다. ( +표준화 가능, +독립적인 프로그래밍 가능 )
2. 서로 관계없는 클래스들에게 관계를 맺어줄 수 있다.
3. 전략 패턴 ( Strategy Pattern )
```

```
## 개발시간을 단축시킬 수 있다 ( +표준화 가능, +독립적인 프로그래밍 가능 ) ##
# method를 호출하는 쪽에서는 method 구현 내용에 관계없이 선언부만 알면 된다.
# 예를 들어, equals()의 구현 내용을 우리가 몰라도, 결과 값이 true / false라는 것을 예상하고 개발을 진행할 수 있다.

# 따라서, interface는 표준이 될 수 있기 때문에 호출하는 쪽과 호출받는 쪽이 독립적인 프로그래밍이 가능하다.
# 이는 결과적으로 개발시간을 단축시킬 수 있는 장점이다.
```
```java
//  개발시간을 단축시킬 수 있다. ( +표준화 가능, +독립적인 프로그래밍 가능 )

//  Comparable을 interface로 선언하여 equals method를 표준화
//  --> String class는 interface 내용을 구현
//  --> StringComp class는 equals method결과 값이 true / false 라는 것을 예상하고 개발 가능
//  --> 실제 자바 코드의 경우, interface에 method input/output에 대해 명시되어 있음 (String)
public interface Comparable<T> {
    public boolean equals(Object object);
}

public class String implements Comparable<String> {
    public boolean equals(Object object) {
        ...
        return true || false;
    }
}

public class StringComp {
    public void method(String var) {
        if("string".equals(var)) {
            ...
        } else {
            ...
        }
    }
}
```
```
## 서로 관계없는 클래스들에게 관계를 맺어줄 수 있다. ##

# Starcraft 게임을 예로 설명
# 건물들이 있고, 특정 건물을 움직일 수 있는 기능을 넣으려고 한다.

# 모든 건물은 Building을 상속받아 구현
# 건물의 종류는 move 가능 여부로 구분
# move O:    Barrack, Factory
# move X:    Academy, Bunker

                        [ Building ]
         ┏━━━━━━━━━━┳━━━━━━━━┻━━━━━━━┳━━━━━━━━━━━┓
    [ Academy ] [ Bunker ]      [ Barrack ] [ Factory ]

# Building에 move관련 method를 추가하면, move불가능한 Academy, Bunker에 영향
# move관련 method를 Barrack, Factory에만 추가 ?
--> 중복되는 코드 발생, 움직일 수 있는 건물이 늘어날 때마다 추가해야 됨
--> 움직이는 건물이 100개인데, 코드 수정 발생 ? 100개 전부 고치는 일 발생

# 위의 2가지 문제를 해결할 수 있는 방법이 interface 구현
```
```java
public interface Moveable {
    //  건물을 올린다.
    public void liftOff();

    //  건물을 이동한다.
    public void move(int x, int y);

    //  건물을 정지한다.
    public void stop();

    //  건물을 착륙시킨다.
    public land();
}

//  Barrack
//  아래와 같이 Barrack, Factory를 구현하면 중복코드도 없어지고, 유지보수의 문제도 사라짐
//  만약, Barrack, Factory의 liftOff, move, stop, land의 기능이 공통으로 같다면,
//  java 8 부터는 interface에 공통 method를 구현할 수 있다.
public class Barrack extends Building implements Moveable {
    ...

    public void liftOff() { ... }

    public void move(int x, int y) { ... }

    public void stop() { ... }

    public void land() { ... }
}
```
```
## Strategy Pattern ( 전략 패턴 ) ##
# 각각의 알고리즘들이 교환 가능하도록 별도로 정의하고, 각각 캡슐화하여 교환하며 사용하는 패턴
# 장점
  1. 코드 중복 방지
  2. Runtime시에 타겟 메소드 변경 ( ? )
  3. 확장성(신규 클래스) 및 알고리즘 변경 용이

# 정의: Client는 다양한 전략 중에 적합한 전략을 생성해 Context에게 전략 객체를 주입한다.

             ┏━━━━━━━━━━━━━━[ Strategy(전략) ]
             ┃
        [ Client ]
             ┃
             ┗━━━━━━━━━━━━━━[ Context ]

# 블로그를 참고하여 말하면,
# Client:   중대장(전략을 Context에게 주입하는 주체)
# Strategy: 총, 칼, 수류탄(전략)
# Context:  군인 (전략을 사용하는 주체)

# Client는 다양한 전략 중에 적합한 전략을 생성해 Context에게 전략 객체를 주입한다.
--> "중대장은 총, 칼, 수류탄 중에 적합한 무기를 생성해 군인에게 지급한다."라고 보면 된다.
```
```java
//  strategy pattern

//  strategy
public interface Strategy {
    public void attack();
}

//  strategy 구현체
public class Gun implements Strategy {
    @Override
    public void attack() {
        System.out.println("Shoooooooothing !");
    }
}

public class Knife implements Strategy {
    @Override
    public void attack() {
        System.out.println("Knife !");
    }
}

public class Grenade implements Strategy {
    @Override
    public void attack() {
        System.out.println("Fire in the hall !");
    }
}

//  전략(무기)를 사용할 컨텍스트
public class Solider {
    void run(Strategy strategy) {
        strategy.attack();
    }
}

//  client (적절한 무기를 지급할 중대장)
public class Captain {
    public void doStrategy() {
        Strategy st = null;
        
        //  전략을 사용할 Context 생성
        //  solider는 strategy instance를 parameter로 받는다.
        Solider solider = new Solider();

        //  전략 선택
        //  1. 다수가 뭉쳐 있을 때, 
        st = new Grenada();
        solider.run(st);

        //  2. 멀리 한 명을 공격할 때,
        st = new Gun();
        solider.run(st);

        //  3. 근접 공격
        st = new Knife();
        solider.run(st);
    }
}

//  결과 값
//  Fire in the hall !
//  Shoooooooothing !
//  Knife !
```

---

## **추상클래스 vs 인터페이스**

---

### ***References***
```
# http://limkydev.tistory.com/84
# http://myeonguni.tistory.com/57
```