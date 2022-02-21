---
title: 06-Vue生命週期與實際應用
description: 可透過生命週期的語法來控制畫面出現的時間
published: true
date: 2021-10-18T03:02:34.477Z
tags: 
editor: markdown
dateCreated: 2021-10-18T03:02:32.527Z
---

# Lifecycle Hooks
vue官網上的生命週期圖:

![image](https://img.onl/vBs9EA)

主要有四個動作，分別是:create、mount、update、unmount，再加上他們被觸發之前或之後的動作。
簡單來說就是妳可以讓 Vue 在不同時間點去執行你希望他做的事。
就比如一個人上班的生命週期可以是:起床前、起床後、到公司前、到公司後、工作完成前、工作完成後、離開公司前、離開公司後。

下面的表格說明:

![image](https://img.onl/rENvDK)

## 筆記

```xml
onBeforeMount :DOM渲染前執行
onMounted :DOM渲染完成後執行
onBeforeUpdate :資料更新，DOM更改前執行
onUpdated :資料更新，DOM更改後執行
onBeforeUnmount :組件銷毀前執行
onUnmounted :組件銷毀後執行
onErrorCaptured :當組件發出錯誤後調用
onRenderTracked :監控virtual DOM 重新選染時調用(此事件告訴你操作什麼監聽了組件，以及該操作的物件)
onRenderTriggered :監控virtual DOM 重新選染時調用(此事件告訴你操作什麼觸發了重新渲染，以及該操作的物件)
```