---
title: 02-Vue的模板語法
description: 
published: true
date: 2021-11-24T01:33:21.531Z
tags: 
editor: markdown
dateCreated: 2021-10-18T02:27:36.197Z
---

# v-on
Vue 的監聽，等同於`.addEvenetListener()`。

```html
<div id="app">
    <h1>{{num}}</h1>
    <h1 style="color: aqua;">{{obj.index}}</h1>
    <button v-on:click="AddFun">Add</button>
    <!-- 完整 -->
    <button @click="RemoveFun">Remove</button>
    <!-- 縮寫 -->
</div>
```

```javascript
<script>
    const { ref, reactive } = Vue;
    const App = {
        setup() {
            const num = ref(0);
            const obj = reactive({index: 0})
            const AddFun = ()=>{
                num.value++ //等同於num.value = num.value + 1
                //ref須加上.value修改資料
                obj.index ++
            }
            const RemoveFun = ()=> num.value--
            return{ num , AddFun, RemoveFun, obj}
        }
    }
    Vue.createApp(App).mount('#app')
</script>
``` 
## 修飾符
![v-on修飾符.png](http://192.168.25.60:8000/files/file_storage/60cf617c.png)

# v-if & v-show
```html
<div id="app">
    <h1 id="vue-if" v-if="isShow">{{text}}</h1>
    <!-- v-if直接移除dom元素與加進dom，頻繁切換時消耗的效能較高 -->
    <h1 id="vue=show" v-show="isShow">{{text}}</h1>
    <!-- v-show 利用控制css 的 display，渲染資料時效號的效能較高-->
    <button @click="HandDomShow">Hand Dom Show</button>
</div>
```
```javascript
<script>
    const { ref } = Vue;
    const App ={
        setup() {
            const text = ref('Hello Vue!')
            const isShow = ref(true)

            const HandDomShow = () =>{
                isShow.value = !isShow.value;
            }
            return{
                text,
                isShow,
                HandDomShow,
            }
        }
    }
    Vue.createApp(App).mount('#app')
</script>
```
當`isShow`為true時開啟，fales時關閉。
可以明顯看出兩者的區別，`v-if`移除或加入dom中；`v-show`是控制css的display。

![v-if示範.gif](http://192.168.25.60:8000/files/file_storage/83c23c26.gif)

# v-else & v-else-if
`v-else`一定要依附在`v-if`的元素之後，不能在中間穿插別的東西，如果不是連在一起就會壞掉。

```html
<div id="app">
    <p id="A"  v-if="showMessage">A</p>
    <p id="B"  v-else>B</p>
</div>
```
`showMessage`若為`true`，render`#A`，若為`false`就render`#B`( 非A即B )

`v-else-if`的邏輯與javascript的條件判斷式一樣。
```html
<div id="app">
    <p id="A"  v-if="showMessageA">A</p>
    <p id="B"  v-else-if="showMessageB">B</p>
    <p id="C"  v-else>C</p>
</div>
```
邏輯是:
`showMessageA = true`，顯示A。
`showMessageA = false, showMessageB = true`，顯示B。
`showMessageA = false, showMessageB = false, showMessageB = true`，顯示C。

# v-for
當html有一些重複的元素，例如列表、清單、下拉選單等等。
可以用`v-for`把陣列或物件的資料轉成元素，render出來。

```javascript
<script>
    const { reactive } = Vue;
    const App = {
        setup(){
            const listArr = reactive([
                { name: '2020 Vue3 專業職人 | 入門篇', show:true},
                { name: '2020 Vue3 專業職人 | 加值篇', show:false},
                { name: '2020 Vue3 專業職人 | 進階篇', show:true},
                { name: '現代Javascript 職人之路 | 入門篇', show:false},
                { name: '現代Javascript 職人之路 | 中階實戰篇', show:true},
            ])
            return {
                listArr,
            }
        }
    }
    Vue.createApp(App).mount('#app')
</script>
```
```html
<div id="app">
    <ul>
        <li v-for="(list, index) in listArr" v-show="list.show">
            {{index + 1}} {{list.name}}
        </li>
    </ul>
</div>
```
使用`v-for="(list, index) in listArr"`，拿掉第二個變數的話是要這樣寫`v-for="list in listArr"`，意思是抓取`listArr`陣列裡的元素，把這個元素指定為`list`，並使用`{{list.name}}`將指定值render出來。
這裡的`index`是陣列的索引值。

# v-bind
屬性綁定，可綁定`class`或`style`。

例如要控制某個按鈕是否為`disabled`的狀態，可以這樣寫:

```javascript
const vm = Vue.createApp({
    data(){
        return{
            isBtnDisabled: true
        }
    },
}).mount('#app')
```
```html
<button v-bind:disabled="isBtnDisabled">click</button>
<!-- 完整 -->
<button :disabled="isBtnDisabled">click</button>
<!-- 縮寫 -->
```
如果`isBtnDisabled`為`true`，那按鈕會掛上`disabled`屬性，無法使用。
如果為`false`，這個按鈕就可以正常使用。

## 舉例:透過布林來控制class的開跟關
範例畫面如下:
![v-bind示範.gif](http://192.168.25.60:8000/files/file_storage/e6d6423a.gif)

css設定:
```css
*{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
} 
html, 
body{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    background-color: slategray;
}
#app {
    width: 400px;
    overflow: hidden;
    border-radius: 10px;
}
.tittle {
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 50px;
    background-color: #42b983;
    font-weight: bold;
    color: darkslategray;
    font-size: 20px;
}
.box{
    display: block;
    width: 100%;
    height: 0; /* 關閉高度為0 */
    background-color:snow;
    transition: height 0.4s;
}
.box.open{
    height: 200px;  /* 加上open時高度為200px */
}
.box > li {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 40px;
    border-bottom: 1px solid #d2d2d2;
    font-size: 12px;
    color: darkslategray;
}
```

Vue.js:
```javascript
const App = {
    setup() {
        const isOpen = ref(true); //預設為開啟狀態
        const listArr = reactive([
            { name:'2020 Vue3 專業職人 | 入門篇' },
            { name:'2020 Vue3 專業職人 | 加值篇' },
            { name:'2020 Vue3 專業職人 | 進階篇' },
            { name:'現代Javascript 職人之路 | 入門篇' },
            { name:'現代Javascript 職人之路 | 中階實戰篇' },
        ]);

        const handOpenClass = () => {
            isOpen.value = !isOpen.value; //轉換開或關
        }
        return {
            isOpen,
            listArr,
            handOpenClass,
        }
    }
}
```

html:
```html
<div id="app">
    <!-- 監聽click事件轉換開或關 -->
    <a @click="handOpenClass" class="tittle">課程列表</a>
    <!-- 原寫法: <ul class="box" v-bind:class="{open: isOpen}"> -->
    <!-- 也可以這樣寫: 當複數個class，可用arry包裹，被控制的class用object包起-->
    <ul :class="['box', {open: isOpen}]">
        <li v-for="(list, idex) in listArr" :key="list.name">
            {{idex + 1}}. {{list.name}}
        </li>
    </ul>
</div>
```
資料參考:
https://book.vue.tw/CH1/1-5-events.html
https://v3.vuejs.org/api/directives.html#v-bind