---
layout: post
title: '[Android] TitleBar'
author: 'Jaeik Lee'
categories: [android]
tags: [android, titlebar, statusbar]
---

---

## **Title Bar 지우기**

```xml
<!-- src/res/values/styles.xml -->
<resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>

        <!-- [ start ]  title bar -->
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <!-- [ end ]    title bar -->

    </style>
</resources>

```

---

## **Status Bar 지우기**

```java
//  src/main/MainActivity.java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    //  [ start ]   status bar
    getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
            WindowManager.LayoutParams.FLAG_FULLSCREEN);
    //  [ end ]     status bar

    setContentView(R.layout.activity_main);
}
```

---

### **_References_**

```
# http://commin.tistory.com/63
```
