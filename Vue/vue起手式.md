---
title: 01-vue起手式與ref & reactive
description: 
published: true
date: 2021-11-24T01:34:48.050Z
tags: 
editor: markdown
dateCreated: 2021-10-18T01:40:16.858Z
---

# Vue 生命週期
![vue生命週期.png](http://192.168.25.60:8000/files/file_storage/1fd43e94.png)

## setup()
html部分:
```html
<body>
    <div id="app"></div>
</body>
```
起手式:
```javascript
const { ref, reactive } = Vue; 
//將ref和reactive包進Vue中
const App = {
    setup() {
        return {};
    }
}

Vue.createApp(App).mount('#app')
//mount() 負責把data掛載到Vue實例渲染成DOM
```

# ref & reactive

## ref
```html
<body>
    <div id="app">
        <h1>{{text}}</h1>
    </div>
</body>
```

```javascript
const { ref } = Vue;
const App = {
    serup(){
        const text = Vue.ref('hellow Vue!') 
        //可定義多個
        const text2 = Vue.ref('morning Vue!')
        const text3 = Vue.ref('good night Vue!')

        setTimeOut(()=>{
            text.value = 'mike' //改值需用.value
        }, 1000)

        return {text} //再吐出來
    }
}
Vue.createApp(App).mount('#app')
```

## reactive
```html
<body>
    <div id="app">
        <h1>{{obj.name}}</h1>
    </div>
</body>
```

```javascript
const { reactive } = Vue;
const App = {
    serup(){
        const obj = reactive({ 
            //只接受物件或陣列這兩種類型的值
            name: 'Zoe',
            age: 18
        })

        return {obj} //再吐出來
    }
}
Vue.createApp(App).mount('#app')
```

## 統整兩者的差別
```xml
ref: 可接受任何型態的資料，但不會物件或陣列內部的屬性變動做監聽。
reactive: 只能接受物件或陣列，可以做深層的監聽，以及訪問資料不需要加 .value。
```

# 資料雙向綁定 v-model
可以對`input` `textarea` `select` `checkbox`等，這些輸入表單做雙向綁定，運作原理是透過監聽使用者輸入的事件來更新內容。

```html
<div id="app">
    <h1>{{text}}</h1>
    <input type="text" v-model='text'>
    <br>
    <br>
    <br>
    <h1>{{obj.name}}</h1>
    <input type="text" v-model='obj.name'>
</div>
```

```javascript
<script>
    const { ref, reactive } = Vue
    const App = {
        setup() {
            const text = ref('hello City!')
            const obj = reactive({ name: 'ZOE',})
            return{ text, obj}
        }
    }
    
    Vue.createApp(App).mount('#app')
</script>
```
`ref`跟`reactive`的綁定方法一樣，再javascript return出去，在html用`v-model`綁定在`input`上，當輸入框改變時回同步渲染在`h1`上。

![v-model示範.gif](http://192.168.25.60:8000/files/file_storage/fde40304.gif)

## 補充用法
- `.trim`
在輸入資料時，有些人一開始前面可能多打了一個空白鍵，或是複製貼上時不小心多選到一個空白，這時`.trim`就能幫忙我們去掉空白~，直接在v-model後加入`.trim`就行了！如下：
```html
<input type="text" v-model.trim="myname" value="myname">
```

- `.lazy`
v-model的例子，我們在`<input>`輸入文字時，它是完全即時同步的，那如果我要等離開`<input>`時才更新我輸入的內容要怎麼做呢?
一樣在v-model後加入`.lazy`就行了！如下：
```html
<input type="text" v-model.lazy="myname" value="myname">
```

- `.number`
輸入欄位中，很多是限定輸入為「數字」的，比如說「年齡」，如果有人不小心輸入了文字怎麼辦?
這時就可以用`.number`，在v-model後加入`.number`，另外將`<input>`設定為type=number，它會只讓「數字」輸入，如下做法：
```html
<input type="number" v-model.number="myname" value="myname">
```
