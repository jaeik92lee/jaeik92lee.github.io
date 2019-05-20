---
layout: post
title: '[Java8] Optinal instead of Null'
author: 'Jaeik Lee'
categories: [java, java8]
tags: [optional, 'null', java, java8]
---

# **Null 대신 Optional**

---

## **_10.1. 값이 없는 상황을 어떻게 처리할까?_**

```java
- 1. sample코드를 바탕으로 작성한 2. find problem에서 코드의 문제점은 어떤 것일까?
- 차를 소유하지 않은 사람이 getCar()를 호출하면? return null
- 그럼, getInsurance()는 null의 보험 정보를 반환하려 하기 때문에 NullPointerException이 발생
- 결과적으로 프로그램 실행이 중단된다.
- 추가적으로 Person이 null인 경우와 getInsurance가 null을 반환할 때도 똑같은 에러가 발생한다.

//  1. sample
public class Person {
    private Car car;
    public Car getCar() { return car; }
}

public class Car {
    private Insurance insurance;
    public getInsurance() { return insuracne; }
}

public class Insurance {
    private String name;
    public String getInsurance() { return name; }
}

//  2. find problem
public String getCarInsuranceName(Person person) {
    return person.getCar().getInsurance().getName();
}

```

```java
- 2의 문제를 해결하기 위해 아래와 같이 해결할 수 있다.
- 3번의 경우는 null인지 의심되는 코드를 접근할 때마다 중첩된 if가 추가되기 때문에 들여쓰기 수준이 증가한다.
- 이와 같은 반복 패턴 코드를 '깊은 의심(deep doubt)'라고 부른다.
- 아래의 단점은 가독성 저하, 코드의 구조 파괴

//  3. resolve problem - 1
public String getCarInsuranceName(Person person) {
    if(person != null) {
        Car car = person.getCar();
        if(car != null) {
            Insurance insurance = car.getInstance();
            if(insurance != null) {
                return insurance.getName();
            }
        }
    }
    return "Unknown";
}

- 4번은 3번의 문제를 해결한 것이다.
- 중첩 if 블록을 없앴다.
- 아래의 코드도 4개의 출구가 생겼기 때문에 유지보수가 어려워진다.

//  4. resolve problem - 2
public String getCarInsuracneName(Person person) {
    String unKnown = "Unknown";

    if(null == person) {
        return unKnown;
    }

    Car car = person.getCar();
    if(null == car) {
        return unKnwon;
    }

    Insurance insurance = car.getInsurance();
    if(null == insurance) {
        return unKnwon;
    }

    return insurance.getName();
}
```

---

## **_10.1.2 null 때문에 발생하는 문제_**

```
1. 에러의 근원
2. 코드를 어지럽힌다.
3. 아무 의미가 없다.
4. 자바 철학에 위배된다 : Java는 개발자로부터 모든 Pointer를 숨겼지만, NullPointer는 숨기지 못했다.
5. 형식 시스템에 구멍을 만든다 : null은 무형식이고, 정보를 포함하지 않으므로 애초에 null이 어떤 의미로 사용되었는지 알 수 없다.
```

---

## **_10.1.3 다른 언어는 null 대신 무얼 사용하나?_**

```groovy
- 그루비는 안전 내비게이션 연산자(?.) 도입
- 아래는 person, car, insurance, name 중 null값이 있으면 null을 반환한다.

def carInsuranceName = person?.car?.insuracne?.name
```

---

## **_10.2 Optional 클래스_**

```java
- java8은 "선택형값" 개념의 영향으로 java.util.Optional<T>라는 새로운 클래스를 제공한다. (하스켈과 스칼라의 영향이라고 한다.)
- Optional의 역할은 더 이해하기 쉬운 API를 설계하도록 돕는 것이다.
- Optional class는 값이 있으면 감싸고, 값이 없으면 Optional.empty 메서드를 반환

- null vs Optional.empty()
- Person class의 car, Car class의 Insurance는 있을수도 없을수도 있으니 Optional로 정의한다.
- Insurance class의 name은 무조건 있다고 가정하고, String으로 정의한다.
- 아래와 같이 정의함으로써, "사람은 차를 소유했을 수도, 아닐수도", "자동차는 보험에 가입되어 있을수도 아닐수도" 있음을 명확히 설명한다.

- 또한, 보험회사의 이름은 String으로 선언되어 있기 때문에, 보험회사는 무조건 이름을 가져야 함을 보여준다.
- 보험회사의 이름을 참조할 때 NullPointerException이 발생할 수도 있다. 하지만, null인지 확인하는 코드를 추가할 필요는 없다.
- 왜? 오히려 문제를 감추는 꼴. 이름이 없는 보험회사를 발견했다면, 이름이 없는 이유를 밝혀서 해결해야 한다.
public class Person {
    private Optional<Car> car;
    public Optional<Car> getCar() { return car; }
}

public class Car {
    private  Optional<Insurance> insurance;
    public Optional<Insurance> getInsurance() { return insurance; }
}

public class Insurance {
    private String name;
    public String getName() { return name; }
}

```

---

## **_10.3 Optional 적용 패턴_**

```java
//  1. 빈 Optional
Optional<Car> optCar = Optional.empty();
log.info(optCar.toString());    //  Optional.empty

//  2. null이 아닌 값으로 Optional 만들기
//  car가 null이면, NullPointerException
//  Optional을 사용하지 않았다면, car property에 접근할 때, 에러가 발생했을 것.
Car car = null;
Optional<Car> optCar = Optional.of(car);
log.info(optCar.toString());    //  NullPointerException

Car car = new Car();
Optional<Car> optCar = Optional.of(car);
log.info(optCar.toString());    //  Optional[car_address]

//  3. null값을 저장하는 Optional
Car car = null;
Optional<Car> optCar = Optional.ofNullable(car);
log.info(optCart.toString());   //  Optional.empty

Car car = new Car();
Optional<Car> optCar = Optional.ofNullable(car);
log.info(optCar.toString());    //  Optional[car_address]

```

---

## **_10.3.2 map으로 Optional 값을 추출하고, 변환하기_**

```java
//  Optional이 값을 포함하면 map의 인수로 제공된 함수가 값을 바꾼다.
//  Optional이 비어있으면 아무 일도 일어나지 않는다.
Insurance insurance = null;
Optional<Insurance> optInsurance = Optional.ofNullable(insurance);
Optional<String> name = optInsurance.map(Insurance::getName);
log.info(optInsurance.toString());  //  Optional.empty
log.info(name);                     //  Optional.empty

Insurance insurance = new Insurance("TEST_INSURANCE");
log.info(optInsurance.toString());  //  Optional[Insurance(name=TEST_INSURANCE)]
log.info(name);                     //  Optional[TEST_INSURANCE]
```

---

## **_10.3.3 flatMap으로 Optional 객체 연결_**

```java
//  아래의 코드를 Java8로 변환해보자.
public String getCarInsuranceName(Person person) {
    return person.getCar().getInsurance().getName();
}

----- stream 공부 후, 업데이트 필요 -----
//  스트림의 flatMap은 함수를 인수로 받아서 다른 스트림으로 반환하는 메서드이다.
//  인수로 받은 함수를 스트림의 각 요소에 적용하면 스트림의 스트림이 만들어진다.
//  flatMap은 인수로 받은 함수를 적용해서 생성된 각각의 스트림에서 콘텐츠만 남긴다.

//  위의 코드를 수정한 Optional 코드
//  결과 Optional이 비어있으면 기본값(Unknown)을 사용한다.
public String getCarInsuranceName(Optional<Person> person) {
    return person.flatMap(Person::getCar)
                .flatMap(Car::getInsurance)
                .map(Insurance::getName)
                .orElse("Unknown");
}

Optional<Person> person = null;
log.info("[ Optional ] : {}", getCarInsuranceName(person)); //  NullPointerException

Optional<Person> person = Optional.ofNullable(null);
log.info("[ Optional ] : {}", getCarInsuranceName(person)); //  Unknown

Optional<Person> person = Optional.empty();
log.info("[ Optional ] : {}", getCarInsuranceName(person)); //  Unknown


//  test - 1
Insurance insurance = new Insurance();
insurance.setName("TEST_INSURANCE");

Optional<Insurance> optInsurance = Optional.ofNullable(insurance);

Car car = new Car();
car.setInsurance(optInsurance);

Optional<Car> optCar = Optional.ofNullable(car);

Person person = new Person();
person.setCar(optCar);

Optional<Person> optPerson = Optional.ofNullable(person);
log.info("[ Optional ] : {}", getCarInsuranceName(optPerson));  //  TEST_INSURANCE

//  test - 2
Optional<Insurance> optInsurance = Optional.ofNullable(null);
log.info("[ Optional ] : {}", getCarInsuranceName(optPerson));  //  Unknown

//  test - 3
Optional<Car> optCar = Optional.ofNullable(null);
log.info("[ Optional ] : {}", getCarInsuranceName(optPerson));  //  Unknown

//  test - 4
Optional<Person> optPerson = Optional.ofNullable(null);
log.info("[ Optional ] : {}", getCarInsuranceName(optPerson));  //  Unknown
```

```java
//  Optional은 데이터 직렬화를 지원하지 않는다.
//  따라서, 직렬화 모델이 필요하다면 아래와 같이 Optional로 값을 반환받을 수 있는 메소드가 필요하다.
public class Person {
    private Car car;
    public Optional<Car> getCarAsOptional() {
        return Optional.ofNullable(car);
    }
}
```

---

### **_References_**

```
# Java8 in Action : Chapter 10.
```
