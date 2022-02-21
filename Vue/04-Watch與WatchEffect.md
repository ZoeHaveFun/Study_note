---
title: 04-Watch與WatchEffect
description: 
published: true
date: 2021-10-18T02:41:30.775Z
tags: 
editor: markdown
dateCreated: 2021-10-18T02:41:11.435Z
---

# Watch起手式
```javascript
watch(被監聽變數, (新值, 舊值) => {
  要做的事...
});
```

## Watch-ref
1.若是監聽`ref`的值，那就直接監聽即可。

```javascript
const index = ref(0)
watch( index, (newIdx, oldIdx) => {
    console.log('new:',newIdx ,'old:',oldIdx );
})
setInterval(() => {
    index.value++
}, 1000)
```

2.若是監聽`ref`物件的值，那要針對其中的資料，並將之改成getter可讀取的值(不然他就報錯給你看)，也就是`()=> refObj.value.idx`

```javascript
const refObj = ref({ idx: 0})
const reactiveObj = reactive({ idx: 0})

watch(
    //無法監聽物件或陣列內部變化，需指定某個值
    () => refObj.value.idx,  //只能得到idx的值
    (newIdx) => {
    console.log('ref:', newIdx);
})
watch(reactiveObj, (newIdx) => {
    console.log('reactive:', newIdx);
    //可監聽整個obj
})
```

3.若是監聽`ref`整個物件，要加入第三個參數來做深層監控，也就是`{deep: true}`才行，無法取得舊值。

```javascript
const data = ref({ user: {}})
//當我1s後，要加入key為'name'，value為'zoe'到user這obj中。

watch(data, (newKey) => {
    console.log(newKey);
}, {deep: true}) 
//deep: true，告知watch需要深度觀察(預設是不會做深度監聽)
//immediate: true，讓watch在初始化完成後就先觸發(預設是監聽的值有變化時才觸發)

watch(data.value.user, (newKey) => {
    console.log(newKey);
})

setInterval(() => {
    data.value.user['name'] = 'zoe'
}, 1000);
return {}
```

結果:

![image](https://img.onl/qG66qp)

重點整理:
`deep`: 當欲監聽值的特性為call by reference，例如obj時，需將deep值定為true，告知watch需要深度觀察，否則預設是不會做深度監聽的。

`immediate`: 監聽器預設為當值有變化時才觸發。如果我們希望在初始化完成後就先觸發，執行裡面的程序，就可以將`immediate`直設為true。

## Watch-reactive
1.若是監聽`reactive`物件的值，那要針對其中的資料，定將之改成getter可讀取的值，也就是`() => reactiveobj.idx`這種方式。

2.若是監聽`reactive`整個物件，無法取得舊值。
```javascript
const reactiveObj = reactive({ idx: 0})

//監聽obj裡idx的值
watch(
    () => reactiveObj.idx,
    (newIdx) => {
    console.log('ref:', newIdx);
})

//監聽整個obj
watch(reactiveObj, (newIdx) => {
    console.log('reactive:', newIdx);
    
})
```

- ### **WatchEffect起手式**

不須傳入欲監聽參數，只要在`{}`中直接使用參數值，就會自動監聽。

```javascript
watchEffect(() => {
  console.log(num.value);
});
```

舉例:

```javascript
const { ref, reactive, watch, watchEffect } = Vue;

const App = {
  setup() {
    const num = ref(0);
    const refObj = ref({ idx: 0 });
    const reactiveObj = reactive({ idx: 0 });

    watchEffect(() => {
      console.log("watchEffect 監控 ref 資料 = " + num.value);
      console.log("watchEffect 監控 ref 物件 = " + refObj.value.idx);
      console.log("watchEffect 監控 reactive 物件 = " + reactiveObj.idx);
    });

    return{};
  }
}    
```

## 總結兩者差異
1.`watch`是需要傳入監聽的數據源，而`watchEffect`是自動收集數據源作為依賴。

2.`watch`預設是不會做深度監控，當然也可以設定`deep`，而`watchEffect`會自動做深度監控。

3.`watch` 可以取得監聽變化前後的值(新值、舊值)，而`watchEffect`沒有，`watchEffect`取得的改變後的值。

4.`watch`是監聽對象改變的時候執行，當然也可以設定`immediate`，而`watchEffect`是默認初始化完成就執行一次(相當於自帶`immediate`)，然後監聽對象改變也會執行。