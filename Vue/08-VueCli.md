---
title: 08-Vue Cli 安裝與建立專案
description: 講解Vue.js Command-Line Interface安裝前置作業與安裝方式，還有怎麼建立專案。
published: true
date: 2022-02-09T07:42:27.108Z
tags: 
editor: markdown
dateCreated: 2021-10-08T00:55:14.912Z
---

# Vue Cli 是什麼

> Vue Cli，全名Vue.js Command-Line Interface。提供開發者快速建置Vue.js專案並整合相關工具練的一套指定列(command-line)工具。


Vue Cli 整合了 webpack 和 webpack-dev-server，讓開發者可以快速地建置一套立即可用的開發環境，同時，也可依開發者需要與其他Plugin整合，如`Bebel`、`ESLint`、`SASS`、`Axios`...等等。

透過內部的`vue-cli-service`服務，可以直接透過`serve`、`bulid`等指令來啟動本機的開發環境、測試，或是專案的打包建置等。

# Vue Cli 的安裝

## 安裝SOP
Vue Cli 是基於 Node.js 的環境上做開發，所以需要先下載 Node.js。
但因 Node.js 有許多版本，可能不同專案會使用不同的 Node.js 版本，這時就需要 NVM(一個版本管管理器) 來管理、切換。 

下圖展示了他們~~錯綜複雜、三角戀愛~~的關係圖。
![vuecli版本控制.png](http://192.168.25.60:8000/files/file_storage/1fd43e94.png)

> nvm 載點 https://github.com/coreybutler/nvm-windows
> 
> 如果你不想要 nvm 管理node.js，確定之後都用同一版本的 node.js 的話，也可以直接載node.js，不透過nvm install。
> node.js 載點 https://nodejs.org/zh-cn/
{.is-info}

- 進入 nvm 載點，點選 Releases，往下滑找到 nvm-setup.zip，點選下載。
![vuecli01.png](http://192.168.25.60:8000/files/file_storage/97c250b3.png)
![vuecli02.png](http://192.168.25.60:8000/files/file_storage/5ab201ca.png)

- 打開zip，開始安裝nvm。建議在D槽建一個SDK資料夾，指定nvm住那。
![vuecli03.png](http://192.168.25.60:8000/files/file_storage/34228bed.png)

- 下一步他會請你指定node.js到時要住的地方，也一樣住在SDK中。
![vuecli04.png](http://192.168.25.60:8000/files/file_storage/34228bed.png)
![vuecli05.png](http://192.168.25.60:8000/files/file_storage/50037f9c.png)
- 現在你可以開啟 Terminal 終端機，輸入`nvm -v`，有出現版本號就表示nvm 安裝完成了。
- 輸入 `nvm list available`查看目前可用版本，LTS為穩定版，CURRENT為最新版但不一定穩定。
- 輸入 `nvm install [你選擇的node.js版本]` (載完後可以用`nvm list` 查看)
- 輸入 `nvm use [你選擇的node.js版本]` ，跟電腦說你要使用這個版本
- 安裝完 node.js 之後，我們終於要來安裝主角 Vue Cli。一樣開啟 Terminal並輸入:
![vuecli06.png](http://192.168.25.60:8000/files/file_storage/e2afe736.png)
- 在看到一連串的安裝過程後，在 Terminal 輸入 `vue -V`(!!V是大寫喔!!)，如果有出現 `@vue/cli [版本序號]` 就表示安裝成功囉。

## 安裝時的踩坑紀錄
#### 1. isntall node.js成功，但輸入`nvm use [你選擇的node.js版本]`，報奇怪的符號且沒有使用成功。

奇怪的符號長這樣:
exit status 1: �z�S���������v���i���榹�ާ@�C 
或這樣
exit status 145: �ؿ����O�Ū��C

解決方式:
1.看不懂奇怪的符號，建議在git bash重新操作，git bash顯示出的exit status會是英文。
2.我遇到兩次，分別是:
權限不足，輸入的指令無效 &rArr; 
搜尋Windows PowerShell，右鍵以系統管理員的身分開啟Terminal，在執行一次指令，成功。

報說file已存在 &rArr; 
這是在筆電發生，我確認node.js版本明明載下來了但就是不能使用。我猜測是原本筆電就有載了node.js 而導致，故我找到先前載的node.js並移除，重下use仍報同樣的錯。最後我移除整個nvm，再重新下載需nvm，並用nvm install node.js，再次輸入`nvm use [node.js 版本]`，成功。

#### 2. 輸入`npm install -g @vue/cli`，報沒有權限，不能install。

解決方式:
1.以系統管理員的身分開啟Windows PowerShell，在執行一次指令，成功。

# 透過Vue Cli 建立專案
## 建立SOP
- 在要放置專案的位置，右鍵開啟Terminanl，輸入`vue create [專案名稱]`
- Vue Cli 會問你要建立 Vue 2.x 或 Vue 3.x 或自行手動選擇版本與相關套件設定的專案，我們點選 `Manually select features`。(這裡的setting01是我先前保存的設定，第一次使用時沒有才是正常的)
![建專案-完成.gif](http://192.168.25.60:8000/files/file_storage/14f94a9a.gif)

> 如果你的畫面像這樣的姊妹，請直接到"建立時的踩坑紀錄"，解決完再回來繼續建專案。
![建專案-錯誤示範.gif](http://192.168.25.60:8000/files/file_storage/16463d45.gif)
{.is-danger}

- 接著Vue Cli 會提供各種安裝的選項，我的安裝選項如下，各位可以參考(接下來就是根據我所選的項目做細部的詢問)。
![vuecli08.png](http://192.168.25.60:8000/files/file_storage/84e5e234.png)
- 選擇Vue.js 3 來套用在專案上
![vuecli09.png](http://192.168.25.60:8000/files/file_storage/cd71e733.png)

- 在vue router中可以使用`history`或`hash`兩種模式，default是`hash`模式
![vuecli10.png](http://192.168.25.60:8000/files/file_storage/8ee334c5.png)
hash mode -> url帶有 # 符號，只有 # 前的url包含在http請求中，如:
![vuecli18.png](http://192.168.25.60:8000/files/file_storage/28e8f69a.png)
history mode -> url完整包含在http請求中，如:
![vuecli19.png](http://192.168.25.60:8000/files/file_storage/834729a5.png)
- 選擇SASS/SCSS node-sass 做CSS預處理器(pre-processor)
![vuecli11.png](http://192.168.25.60:8000/files/file_storage/13056ebb.png)

- 選擇 ESLint 來檢查程式碼品質，Prettier統一codeing style
![vuecli12.png](http://192.168.25.60:8000/files/file_storage/8d32c873.png)

- 檢查程式碼的時機，選擇存檔時檢查
![vuecli13.png](http://192.168.25.60:8000/files/file_storage/a867310c.png)

- 把Babel、PostCSS、ESLint等等設定檔放在 package.json
![vuecli14.png](http://192.168.25.60:8000/files/file_storage/be096606.png)

- 詢問是否要儲存這次設定(就如我儲存的setting01)
![vuecli15.png](http://192.168.25.60:8000/files/file_storage/cca517b1.png)

- 接下來就會會跑跑跑，出現下面這個畫面就成功啦，你可以依照指示`cd [專案名稱]`，再輸入`npm run serve`
![vuecli16.png](http://192.168.25.60:8000/files/file_storage/5af1aebc.png)

- 點選連結就可以看到專案的虛擬serve
![vuecli17.png](http://192.168.25.60:8000/files/file_storage/01c0569e.png)
![建專案-serve畫面.gif](http://192.168.25.60:8000/files/file_storage/cd7127b9.gif)

## 建立時的踩坑紀錄
#### 1. 選擇feature的狀態很奇怪
別人按上下鍵是會有select，我按上下只有 | 在動，還要自已刪掉 > ，加到要的選項，總之就是很奇怪。

解決方式:
1.到 Microsoft Store，安裝 Windows Terminal
2.啟動後，在頁籤找到 v 並按下，點擊'設定'
3.在左下角點擊'開啟JSON檔案'
4.找到`profiles`，在`profiles`的`list`array中加入下面的程式碼
```javascript
{
                "acrylicOpacity": 0.94999999999999996,
                "bellStyle": "window",
                "closeOnExit": "graceful",
                "colorScheme": "Campbell",
                "commandline": "C:/Program Files/Git/bin/bash.exe --login -i",
                "cursorColor": "#FFFFFF",
                "cursorShape": "bar",
                "font": 
                {
                    "face": "Consolas",
                    "size": 14
                },
                "historySize": 9001,
                "icon": "C:/Program Files/Git/mingw64/share/git/git-for-windows.ico",
                "name": "Git Bash",
                "padding": "0, 0, 0, 0",
                "snapOnInput": true,
                "startingDirectory": "%USERPROFILE%",
                "tabTitle": "Git Bash",
                "useAcrylic": true
            },
```
5.這樣就可以使用git bash，如要設為預設，可在'啟動'中，將'預設設定檔'改為git bash
6.如此，在選擇feature時就是正常的狀態了

# 指令筆記
## NVM 指令表
- `nvm list` &rArr; 列出目前電腦有安裝的 node.js 版本
- `nvm list available` &rArr; 目前網路上可用的node.js版本列表
- `nvm install 14.18.0` &rArr; 該 node.js 版本下載安裝
- `nvm uninstall 14.18.0` &rArr; 移除該node.js 版本
- `nvm use 14.18.0` &rArr; 使用該 node.js 版本
- `nvm -v` &rArr; 顯示目前該nvm的版本 

## Vue Cli 指令表
- `vue -V` &rArr; 目前使用的Vue Cli版本
- `npm install -g @vue/cli ` &rArr; 安裝 Vue Cli
- `npm update -g @vue/cli` &rArr; 升級全局的 Vue Cli 
- `npm install vue@next` &rArr; 升級到最新版的Vue.js

