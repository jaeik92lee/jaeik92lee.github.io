---
layout: post
title: '[Java8] Stream'
author: 'Jaeik Lee'
categories: [java8]
tags: [stream, java, java8]
---

# **Stream**

---

### **_사용되는 기본 데이터_**

```java
List<Dish> menu = Arrays.asList(
    new Dish( ... ),
    new Dish( ... ),
    new Dish( ... ),
    new Dish( ... ),
    new Dish( ... ),
    new Dish( ... ),
    new Dish( ... );
)

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Dish {
    private String name;
    private String vegetarian;
    private int calories;
    private Type type;
}
```

---

## **_4.1 스트림이란 무엇인가?_**

```
# 데이터 컬렉션 반복을 멋지게 처리하는 기능
```

```java
//  example
//  저칼로리의 요리명을 반환하고, 칼로리를 기준으로 요리를 정렬하는 코드
//  Java 7
List<Dish> lowCaloricDishes = new ArrayList<>();
for(Dish dish : menu) {
    if(dish.getCalories() < 400) {
        lowCaloricDishes.add(dish);
    }
}

Collections.sort(loCaloricDishes, new Comparator<Dish>() {
    public int compare(Dish d1, Dish d2) {
        return Integer.compare(d1.getCalories(), d2.getCalories());
    }
});

List<String> lowCaloricDishesName = new ArrayList<>();
for(Dish dish : lowCaloricDishes) {
    lowCaloricDishesName.add(dish.getName());
}

//  Java 8
List<String> lowCaloricDishesName = menu.stream()
                .filter(dish -> dish.getCalories() < 400)   //  400 칼로리 이하 요리 선택
                .sorted(coparing(Dish::getCalories))    //  칼로리로 정렬
                .map(Dish::getName)     //  요리명 추출
                .collect(toList());     //  리스트 반환
```

---

## **_4.2 스트림 시작하기_**

```java

```

---

### **_References_**

```
# Java8 in Action : Chapter 10.
```
