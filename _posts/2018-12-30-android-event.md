---
layout: post
title: '[Android] Event'
author: 'Jaeik Lee'
categories: [android]
tags: [android, delegate, hierarchy]
---

## **스마트 폰 이벤트**

```
# 스마트 폰에서 발생하는 이벤트는 크게 2가지
# deletagion event: 뷰에서 발생하는 이벤트
# hierarchy event   액티비티에서 발생하는 사용자의 터치나 키 이벤트 처리
# 무슨 차이지?
```

---

## **Delegation Event**

```
# 이벤트 소스와 이벤트 핸들러를 리스너로 연결하여 처리하는 구조
# Event Source:     event가 발생한 view
# Event Handler:    event처리 내용을 가지는 객체
# Listener:         소스와 핸들러를 연결하는 작업

# delegation 이벤트는 뷰에서 터치가 일어났기 때문에 발생하는 것일텐데, 사용자의 터치를 처리하는 hierarchy 이벤트로 처리하지 않는지?
--> 이벤트를 조금 더 명료하게 처리하기 위해
--> 하나의 view에는 여러 개의 터치 이벤트가 있을 수 있기 때문에 어떤 터치 이벤트가 일어났는지 더 명확하게 하기 위함
```

---

## **Delegation Event Flow**

```java
Checkbox vibrateCheckView;

//  1. event 발생
//  setOnXXXListener: 이벤트 소스와 이벤트 핸들러를 리스너로 연결
vibrateCheckView.setOnCheckedChangeListener(new MyEventHandler());

//  2. Listener로 등록된 handler 실행
class MyEventHandler implements CompoundButton.OnCheckedChangeListener {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        //  event 처리
    }
}
```

---

## **Hierarchy Event Model**

```
# 사용자의 키 이벤트, 화면 터치 이벤트를 처리
# 화면을 터치해서 손가락으로 상하좌우 어떤 방향으로 밀었는지 알아낼 때
# 화면 하단의 뒤로가기 등 스마트 폰의 키를 눌렀을 때(뒤로가기, 홈, 오버뷰 버튼)
```

---

### **_References_**

```
# 깡샘의 안드로이드 프로그래밍
```
