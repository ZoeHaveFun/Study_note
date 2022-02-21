---
title: 10-VueRouter路由
description: 
published: true
date: 2022-01-27T00:53:37.968Z
tags: 
editor: markdown
dateCreated: 2021-10-20T01:36:05.597Z
---

# Vue Router 路由
Vue Router 是 Vue.js 官方提供的前端路由管理器
撰寫心得的版本: Vue 3.x  /  Vue Router v4.x

## Vue Router 安裝
1. 用 Vue-CLI 建立專案，選取 `Router`選項

![router.png](http://192.168.25.60:8000/files/file_storage/91c267ac.png)

2. 如果是已建立的專案，只需要打開終端機並切換到專案目錄下。

```javascript
//有安裝 Vue CLI 的情況
$ vue add router@next
```

3. 環境不是 Vue-CLI 的話，也可透過 npm 或 yarm 來進行安裝。

```javascript
//有安裝 Vue CLI 的情況
$ vue add router@next
```

4. CDN方式在網頁引入 `<script>` 標籤。

```html
<script src="https://unpkg.com/vue-router@4"></script>
```

---

安裝完成後，在 `main.js` 透過 `use(router)` 加入至 `app`。
```javascript
// main.js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

const app = createApp(App);
app.use(router);
app.mount("app");
```

---

## 起手式

`<router-link>` 產生對應的連結，在編譯後會變成 `<a>` 標籤。

`<router-view>` 作為Route 畫面的進入點

```html
<div id="app">
  <p>
    // 使用 to 處理連結url
    <router-link to="/">Login</router-link>
    <router-link to="/checkin">Checkin</router-link>
  </p>
  
  // 渲染 route 的位置
  <router-view></router-view>
</div>
```

```javascript
// router.js
import { createRouter, createWebHashHistory } from "vue-router"; //需引入
import Login from "./components/Login.vue";

const routes = [
  {
    path: "/",
    name: "Login",
    component: Login,
  },
  {
    path: "/checkin",
    name: "Checkin",
    component: () => import("./components/Checkin.vue"),
  },
];

const router = createRouter({
  history: createWebHashHistory(),
  routes,
});

export default router;

```

---

## 路由比對失敗(404 error page)

當使用者操作不正常，透過不存在的url進行連結時(不小心手殘在正確的網址後多加東西)，我們可以透過 `/:pathMatch(.*)*` ，來指定所有非其他 path 的路由都會連到這個元件。



```javascript
{
    path: "/:pathMatch(.*)*",
    name: "NotFoundPage",
    component: () => import("./components/NotFoundPage.vue"),
},
```

這樣就能確保使用者嘗試進入不在規則內的 path 都會導到 NotFoundPage 的頁面。


## 錯誤紀錄

### build 完，訪問空白畫面，報說 css 和 js 找不到路徑的錯誤。

原因:
打包後dist目錄下的檔案引用路徑不對，會因找不到檔案而報錯導致空白頁

解決方式:
在 src 資料夾同層目錄建立 vue.config.js 的文件，修正根目錄錯誤問題

將下面內容放進去

```javascript
module.exports = {
  lintOnSave: false,
  publicPath: process.env.NODE_ENV === "production" ? "./" : "/",
};
```
