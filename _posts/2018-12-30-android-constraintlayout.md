---
layout: post
title: '[Android] ConstraintLayout'
author: 'Jaeik Lee'
categories: [android]
tags: [android, layout, constraint, constraintLayout]
---

## **기본 개념**

```
# `16년 google i/o에서 발표
# Layout안의 View의 위치를 상대적으로 지정
```

---

## **기본 속성 예제**

```xml
<!--
    # 기본 개념
    # view의 bottom을 parent의 bottom에 맞춘다.
    # 속성으로 parent가 아닌 다른 View의 id도 가능
-->
<TextView
    android:id="@+id/tv_first"
    app:layout_constraintBottom_toBottomOf="parent"/>
<TextView
    app:layout_constraintLeft_toRightOf="@id/tv_first"/>



<!--
    # View 가운데 정렬
    # View의 왼쪽을 부모의 왼쪽에 맞추고, View의 오른쪽을 부모의 오른쪽에 맞춘다.
-->
<TextView
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"/>


<!--
    # 치우침(bias)
    # 가운데 정렬 후, 어느 한쪽으로 치우치고 싶을 때
        # horizontal_bias: 가로 치우침
        # vertical_bias: 세로 치우침
    # 아래의 코드는, 왼쪽에서부터 0.2만큼 치우치겠다.
-->
<TextView
    app:layout_constraintHorizontal_bias="0.2">


<!--
    # 비율(ratio)
    # 뷰의 크기를 가로세로 비율에 의한 지정 (dimensionRatio)
    # 아래의 예제는 가로크기만큼 1:1로 세로높이도 지정
-->
<TextView
    android:layout_width="wrap_content"
    android:layout_height="0dp"
    app:layout_constraintDimensionRatio="1:1"/>
```

---

### **_References_**

```
# 깡샘의 안드로이드 프로그래밍
# layout chain (가로로 나란히): https://medium.com/@futureofdev/android-constraintlayout-%EC%89%BD%EA%B2%8C-%EC%95%8C%EC%95%84%EA%B0%80%EC%9E%90-62d2ded79c17
```
