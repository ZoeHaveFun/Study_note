---
title: PrimeVue 學習心得
description: 使用環境:VueCli/ vue 3.0.0 使用版本:  primevue 3.10.0 /primeflex 3.1.2 /primeicons 5.0.0
published: true
date: 2022-01-20T03:12:50.374Z
tags: 
editor: markdown
dateCreated: 2022-01-11T00:55:53.684Z
---

# 起手式
## Module Loader 模板加載
- **npm install，將套件安裝到專案中**
```javascript
npm install primevue@^3.10.0 --save
npm install primeicons
npm install primeflex
```
可以到 package.json > depandencies 確認套件與版本
![package.png](http://192.168.25.60:8000/files/file_storage/2070f2fa.png)

> **套件**：套件其實是一個形容詞，形容非原生的、其他人開放的原始碼，可以想像成他是一個零件，可以一個一個拼湊出完整的機器。前端是一個集合名詞，他之所以這麼多功能那麼肥大就是因為他是許多人一起開發出來的心血集合，除非你就只想用  HTML / CSS / JS 開發
{.is-info}


- **設置 PrimeVue 配置**
```javascript
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from 'primevue/config'; //引入
const app = createApp(App);

app.use(PrimeVue); //使用
```

- **然後從 library 引入 component**
```javascript
import {createApp} from 'vue';
import App from './App.vue';
import PrimeVue from 'primevue/config';
import Dialog from 'primevue/dialog'; //引入component
const app = createApp(App);

app.use(PrimeVue);

app.component('Dialog', Dialog); //登記使用
```

- **終於，我們可以使用 component 到專案中**
```javascript
<Dialog></Dialog> 
```

## Script Tag 腳本標籤
直接再瀏覽器中使用：
```javascript
<html>
    <head>
        <meta charset="utf-8">
        <title>PrimeVue Demo</title>
        <link href="https://unpkg.com/primevue/resources/themes/lara-light-indigo/theme.css" rel="stylesheet">
        <link href="https://unpkg.com/primevue/resources/primevue.min.css" rel="stylesheet">
        <link href="https://unpkg.com/primeicons/primeicons.css" rel="stylesheet">

        <script src="https://unpkg.com/vue@next"></script>
        <script src="https://unpkg.com/primevue/core/core.min.js"></script>
        <script src="https://unpkg.com/primevue/slider/slider.min.js"></script>
    </head>

    <body>
        <div id="app">
            <p-slider v-model="val"></p-slider>
            <h6>{{val}}</h6>
        </div>

        <script>
            const {createApp, ref} = Vue;

            const App =  {
                setup() {
                    const val = ref(null);

                    return {
                        val
                    };
                },
                components: {
                    'p-slider': primevue.slider
                }
            };

            createApp(App).use(primevue.config.default).mount("#app");
        </script>
    </body>
</html>
```

## Style 風格
PrimeVue 提供多個免費主題供使用，一樣在 main.js 中引入。

- 引用方法:
```javascript
import "primevue/resources/themes/saga-blue/theme.css  "; //theme
import "primevue/resources/primevue.min.css"; //core css
import "primeicons/primeicons.css"; //icons
```

- 免費主題:
![themes.png](http://192.168.25.60:8000/files/file_storage/0a901e80.png)

- PrimeFlex 引用:
```javascript
import "primeflex/primeflex.css";
```

# OptionsAPI 與 CompositionAPI

## 使用前準備
- 當你已選擇要用的 Component ，需先在 main.js import 並 use ，才能正常使用。
```javascript
import Accordion from "primevue/accordion";
import AccordionTab from "primevue/accordiontab";

app.component("Accordion", Accordion);
app.component("AccordionTab", AccordionTab);
```

- 在要使用的頁面中作為標籤使用。
```html
<template>
  <div class="box-css">
    <h1 class="h1-tittle">Default</h1>
    <Accordion :active="0">
      <AccordionTab header="Header I">
        <p>Lorem ...</p>
      </AccordionTab>
      <AccordionTab header="Header II">
        <p>Sed ...</p>
      </AccordionTab>
      <AccordionTab header="Header III">
        <p>At ...</p>
      </AccordionTab>
    </Accordion>
  </div>
</template>
```

# Components 筆記
紀錄 PrimeVue 組件的使用方法與屬性。


## Messages
提示或警告訊息。

### Toast

- 有固定撰寫方式才能產生此組件。

1. 需引入 `ToastService` 與 `Toast` 才能正常使用喔。

```javascript
import ToastService from "primevue/toastservice";
import Toast from "primevue/toast";

app.use(ToastService);
app.component("Toast", Toast);
```

2. 在欲使用的頁面引入 `ToastService` 的方法 `useToast`
```javascript
import { useToast } from "primevue/usetoast";

export default {
	setup() {
  	const toast = useToast();
    toast.add({
    	severity: "success", //訊息層級(顏色)
      summary: "Success", //訊息標題
      detail: "Click Success", //訊息內容
      life: 3000, //存在時長
    })
  }
}
```
**severity** 嚴重度：有四類 success、info、warn、error 

![toast.png](http://192.168.25.60:8000/files/file_storage/e4073a43.png)

3. 在 `<template>` 中要加入 `<Toast />` 不然訊息不會出來的啦。

```html
<template>
  <Toast />
</template>
```

---

## Form
組成表單的組件分類，包含輸入框、下拉選單、勾選項目等等。

### AutoComplete

1. 特性

**dropdown**：向下按鈕。

**Suggestions**：陣列，用 v-bind 的來綁定。

**field**：要顯示的屬性的名稱。


2. style

**p-float-label**：要做出飄移的標題，需使用此 class，並加上 `<label>` 。

---

### InputNumber

**useGrouping**：布林值，分位符，預設為 fales。

**minFractionDigits**：數字，小數點設置，值為0則不會有小數點後為;值為2則設置小數點後兩位。

**mode**：字串，有兩個類別：decimal (十進制)、currency (貨幣)。
使用 currency 方法，需附加 `currency="國幣簡稱"` 。

EX
```html
<InputNumber v-model="value11" mode="currency" currency="USD" />
```

**showButtons**：布林值，同側上下按鈕做增減，布局可由 `buttonLayout` 操作。

**buttonLayout**：字串，按鈕的布局，有三類：stacked (default) 堆疊 、 horizontal 水平 、 vertical 垂直。

---
## Menu
菜單、導航列。Menuitemu有固定寫法:

```javascript
const item : [
  {
    label: "Home", //標籤、名稱
    icon: "pi pi-home", //prime Icons
    to: "/", //內部路由
    url: "https://www.google.com", //外部鏈接
    command: () => {} //當點擊時執行call back
    items: [
      {子項目/需要的內容是一樣的}
    ]
  }  
]
```
### Steps

- 預設 `:readonly="true"` ， 可用按鈕與監聽函示控制到下一步(如官網範例)。
設定 `:readonly="false"` ， 可點選 steps 切換下面資訊，模式跟 TabMenu 相似 

![tabmenu.gif](http://192.168.25.60:8000/files/file_storage/8cd1a006.gif)

```html
<div class="box-css">
  <h1 class="h1-tittle">Step</h1>
  <Steps :model="stepItems" :readonly="false"> </Steps>
  <router-view /> 
  //需在 router.js 設定 path 與引用 component
</div>
```
router.js 的設定
![tabmenu.gif](http://192.168.25.60:8000/files/file_storage/0f35f0e4.png)

router.js 的設定主要是下方資訊模板的呈現，而上方的步驟順序要用 `:model="陣列"` 綁定產生。
陣列範例如下:

```javascript
const stepItems = ref([
      {
        label: "Personal",
        to: "/menu",
      },
      {
        label: "Seat",
        to: "/menu/seat",
      },
      {
        label: "Payment",
        to: "/menu/payment",
      },
      {
        label: "Confirmation",
        to: "/menu/confirmation",
      },
]);
```
---

### MegaMenu 
- 是一起顯示子選單的 menu， items 的寫法如下:

```javascript

```

![megamenu.gif](http://192.168.25.60:8000/files/file_storage/39dd5346.gif)

---

## Panel
為一容器可容納其他元件，可做為分頁、收合、動態產生卡片的形式呈現。

### Accordion 手風琴

1. 手風琴內的子項目為 `<AccordionTab>`。

子項目的標題用 `header="字串"` 寫入。
`disabled="布林值"` 可讓某個子項目為禁用(點擊不能打開、只能看到標題且透明度較低)。

```html
<Accordion>
 <AccordionTab header="Header I">
  //內文
 </AccordionTab>
 <AccordionTab header="Header II">
  //內文
 </AccordionTab>
 <AccordionTab header="Header III" disabled="true">
  //內文
 </AccordionTab>
</Accordion>
```
2. Accordion 的特性。

**activeIndex**：數字，預設打開狀態的索引值，需用 v-bind 使用。
EX `:activeIndex="0"` 第一個項目預設為 active (打開狀態)

**multiple**：布林值，預設為 `false` ，不能同時打開多個子項目。

---

### Card 卡片
1. Slots，寫在 `<template>` 中用 `#` 為開頭。

```html
<Card style="width: 25rem" class="bg-gray-200">
      <template #header> header </template>
      <template #title> title </template>
      <template #subtitle> subTitle </template>
      <template #content>
        <p>
          Quaerat velit incidunt quibusdam magnam qui accusantium, aliquid
          architecto ratione voluptatum veniam praesentium? Ex veniam earum
          dicta illum!
        </p>
      </template>
      <template #footer>
        <Button icon="pi pi-check" label="Save" />
        <Button
          icon="pi pi-times"
          label="Cancel"
          class="p-button-secondary ml-1"
        />
      </template>
    </Card>
```
呈現畫面:
*這裡是特別用顏色來凸顯各插件的範圍~

![card.png](http://192.168.25.60:8000/files/file_storage/5cd4a69b.png)

---

### Fieldset 自段集

1. 標題是用 `legend="字串"` 加入。
2. **toggleable**：布林值，點擊 "legend" ，可開關內容。
3. **collapsed**：布林值，預設為開 (true) 或關 (false)。

```html
<Fieldset legend="Header" toggleable="true">
	<p>
  	Lorem ipsum dolor sit amet consectetur adipisicing elit. Fuga esse
    aperiam a nostrum doloribus beatae vel suscipit, ipsa illo et
    commodi atque minima voluptatum alias, blanditiis assumenda nam nisi voluptas.
	</p>
</Fieldset>
```

![field.gif](http://192.168.25.60:8000/files/file_storage/9da4e016.gif)


---
### TabView 頁籤

1. 由父標籤 `<TabView>` 與 子標籤 `<TabPanel>` 組成。

2. 標題由 `header="字串"` 放在 `<TabPanel>` 中。

---



## Overlay 
遮罩層，或稱為彈跳視窗，需有事件觸發組件產生。

### ConfirmDialog 確認對話框

- 有固定撰寫模式才能產生此組件。
1. 需引入 `ConfirmationService` 與 `ConfirmDialog` 才能正常使用喔，不然 Vue 會說他不認識。

```javascript
import ConfirmationService from "primevue/confirmationservice";
import ConfirmDialog from "primevue/confirmdialog";

app.use(ConfirmationService); /* 注意這裡是用use喔 */
app.component("ConfirmDialog", ConfirmDialog);

```

2. 在要使用的頁面引入 `ConfirmationService` 的方法 `useConfirm`。
詳細寫法如下：

```javascript
import { useConfirm } from "primevue/useconfirm";

export default ({
    setup() {
        const confirm = useConfirm();
        confirm.require({
            header: '確認對話框',
            message: '這是訊息的詳細內容不拉不拉',
            icon: 'pi pi-exclamation-triangle',
            accept: () => {
                //callback to execute when user confirms the action
            },
            reject: () => {
                //callback to execute when user rejects the action
            }
        });
    }
})
```

3. 在 `<template>` 中，記得要放 `<ConfirmDialog>` ，不然怎麼點他都不會裡你喔(你都沒把他放上去他是要怎麼出現啦)

```html
<div class="box-css">
	<h1 class="h1-tittle">Overlay</h1>
	<Toast /> //這裡我多加Toast，點擊 yse/no 時跳出相應的訊息
	<ConfirmDialog></ConfirmDialog>
  
	<div>
		<Button
		@click="confirm1()"
		label="Basic"
		icon="pi pi-hashtag"
		></Button>
	</div>
</div>
```

![confirm.png](http://192.168.25.60:8000/files/file_storage/d51e960f.png)

- 客制化選項 Customized Options

```javascript
confirm.require({
  header: "改變按鈕style",
  message: "新增選項可改變accept和reject按鈕style",
  icon: "pi pi-exclamation-circle",
  acceptClass: "p-button-success", //顏色
  acceptLabel: "確定", //按鈕標籤
  acceptIcon: "pi pi-check", //按鈕圖標
 	position: "top", //確認框出現的位置 
  accept: () => {},
	reject: () => {},
});
```

![confirm2.png](http://192.168.25.60:8000/files/file_storage/a9fce2f8.png)

---

### Tooltip

- 引用寫法

```javascript
import Tooltip from 'primevue/tooltip';

app.directive('tooltip', Tooltip);
```

- 使用方法:
預顯示的 message 用 `v-tooltip="要顯示的訊息資訊"` ，可適用在 `<InputText>` 、 `<Button>` 等 component 中。
```html
<InputText type="text" v-tooltip="'輸入密碼不拉不拉'" />

<InputText type="text" v-tooltip.bottom="'訊息在下'" />
<InputText type="text" v-tooltip.top="'訊息在上'" />
<InputText type="text" v-tooltip.left="'訊息在左'" />
<InputText type="text" v-tooltip.right="'訊息在右'" />

<InputText type="text" v-tooltip.bottom.focus="focus狀態顯示訊息" />
```

![confirm2.png]()


---
