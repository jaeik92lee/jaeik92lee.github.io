---
layout: post
title: '[SpringBoot] BuildTool'
author: 'Jaeik Lee'
categories: [spring boot]
tags: [spring boot, build, maven, gradle]
---

```
# Spring Boot에서 Gradle과 Maven의 차이를 알아본다.
```

---

## **들어가기 전에**

```
# 빌드란 ?
 - 소스코드를 컴퓨터에서 실행할 수 있도록 변환하는 과정
 - 빌드는 compile, testing, inspection, deploy의 과정을 포함한다.
 - 즉, 소프트웨어를 생성하고 테스트하고 검사하여 배포하기 위해 수행하는 행위의 집합
 - 빌드는 자동화된 절차에 따라 수행할 수 있어야 한다.

# 빌드 도구란 ?
 - ant, maven, gradle과 같이 빌드를 도와주는 tool
```

---

## **Maven vs Gradle**

```
# 두 개의 차이점에 대해 아직 이해하지 못했다.
# 알아가며 추가하기

# maven
 - xml

# gradle
 - groovy
```

---

## **결론**

```
# gradle은 maven보다 적어도 2배 이상 빠르다. ( gradle v3.5 이상 )
# gradle은 maven보다 간결하고, 가독성이 좋다.
# gradle은 상속구조를 이용한 멀티 모듈 구현이 가능하다.
# gradle은 특정 설정을 소수의 모듀레 공유하기 위해서는 부모 프로젝트를 생성해서 상속해야함 ( 단점 )
```

---

### **_References_**

```
# https://www.holaxprogramming.com/2017/07/04/devops-gradle-is-faster-than-maven/
# https://blog.naver.com/PostView.nhn?blogId=seatom&logNo=220881024126&categoryNo=0&proxyReferer=&proxyReferer=http%3A%2F%2Fblog.naver.com%2FPostView.nhn%3FblogId%3Dseatom%26logNo%3D220881024126%26categoryNo%3D0%26parentCategoryNo%3D0%26viewDate%3D%26currentPage%3D1%26postListTopCurrentPage%3D1%26from%3DpostView
```
