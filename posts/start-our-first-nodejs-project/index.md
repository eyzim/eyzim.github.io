# [NodeJS] 程式麻瓜的第一個 nodeJS 專案



## 開始之前

這篇文章在環境設定的部分是以 Windows 做為示範，若使用 MacOS，請直接跳至 [建立乾淨的專案](#%E5%BB%BA%E7%AB%8B%E4%B9%BE%E6%B7%A8%E7%9A%84%E5%B0%88%E6%A1%88)


確定電腦環境中有安裝這些:

{{< admonition type=info title="安裝軟體">}}
1. [Visual Studio Code](https://code.visualstudio.com/)
2. Windows Powershell
3. [NodeJS](https://nodejs.org/zh-tw/download/)
{{< /admonition >}}

其中 VSCode 可以是任何其他你已經相當習慣的文字編輯軟體 ~~Word~~。

### 安裝

1. Visual Studio Code 
直接安裝 stable version，一直點確定確定確定就安裝好了。

    {{< image src="/images/start_first_nodejs/download_vscode.png"  height="200" 
            src_s="start_first_nodejs/download_vscode.png" 
            src_l="start_first_nodejs/download_vscode.png" 
    alt="download_vscode" caption="Visual Studio Code" linked="false">}}

2. Windows Powershell 則可以在 Windows 左下角搜尋欄中輸入 "PowerShell" 找到。

    {{< image src="/images/start_first_nodejs/look_for_powershell.png"  height="200" 
            src_s="start_first_nodejs/look_for_powershell.png" 
            src_l="start_first_nodejs/look_for_powershell.png" 
    alt="look_for_powershell" caption="PowerShell" linked="false">}}

3. 下載 node.js LTS 版本
    {{< image src="/images/start_first_nodejs/downlaod_nodejs.png"  height="200" 
            src_s="start_first_nodejs/downlaod_nodejs.png" 
            src_l="start_first_nodejs/downlaod_nodejs.png" 
    alt="downlaod_nodejs" caption="NodeJS" linked="false">}}

## 建立乾淨的專案

1. 開啟 VSCode，開啟一個乾淨的資料夾。

2. 接下來我們會在這個資料夾中新增很多小專案，所以再新增第一個麻瓜資料夾，萬一被麻瓜法術搞砸，刪掉這個資料夾就一切和平了

    * 開啟 VSCode 的 terminal

        1. 在 VSCode 乾淨的畫面上按 `ctrl` + `p`

        2. 輸入 `>terminal`

        3. 選擇 `JavaScript Debug Terminal`

            {{< image src="/images/start_first_nodejs/open_vscode_terminal.png"  height="200" src_s="start_first_nodejs/open_vscode_terminal.png"  src_l="start_first_nodejs/open_vscode_terminal.png" alt="open_vscode_terminal" caption="Open the terminal of VSCode" linked="false">}}

        4. 此時 VSCode 視窗下半部就會彈出小視窗了視窗了

            {{< image src="/images/start_first_nodejs/vscode_terminal.png"  height="200" src_s="start_first_nodejs/vscode_terminal.png"  src_l="start_first_nodejs/vscode_terminal.png" alt="vscode_terminal" caption="This is the terminal of VSCode" linked="false">}}

    - 新增子資料夾 `first-nodejs-app`
    
        ```bash
        # 新增資料夾
        mkdir first-nodejs-app

        # 移動到資料夾中
        cd first-nodejs-app
        ```

## 開始 NodeJS 的大門

1. 第一個檔案 `index.js`

    ```bash
    touch index.js
    ```

2. 建構整個網頁的配置檔 npm 管理工具
    nodejs 中，所有需要的延伸工具包，都是由 npm 這個指令所管理

    ```bash
    npm init
    ```

    接著會有許多問你是不是對不對的問題，如果覺得麻煩，也可以這樣

    ```bash
    npm init -y
    ```

    所有問題都直接代替你回答 yes 了

3. 安裝我們的第一個工具包
    這個工具包讓我們執行 nodejs 時，不用每次修改後都要重啟 nodejs，永遠會為你更新到現在修改完的成果

    ```bash
    npm i express
    npm i path
    npm i ejs
    ```

    這裡的 `-g` 代表著安裝到本機端，如果不要加上 `-g`，每次我們新增專案時都要再一次安裝這個工具

    ```bash
    npm i -g nodemon
    ```

    - 嘗試使用 nodemon

        ```
        nodemon index.js
        ```

        這時出現了

        ```txt
        first-nodejs-app> nodemon .\index.js
        nodemon : 因為這個系統上已停用指令碼執行，所以無法載入 C:\Users\eyzim\AppData\Roaming\npm\nodemon.ps1 檔案。如需詳細資 
        訊，請參閱 about_Execution_Policies，網址為 https:/go.microsoft.com/fwlink/?LinkID=135170。
        位於 線路:1 字元:1
        + nodemon .\index.js
        + ~~~~~~~
            + CategoryInfo          : SecurityError: (:) [], PSSecurityException
            + FullyQualifiedErrorId : UnauthorizedAccess
        ```


### PowerShell 處理權限問題

最後一行寫著 `UnauthorizedAccess`，看起來是權限不足

1. 一樣在 Windows 畫面左下角搜尋 `PowerShell`
    
2. 以系統管理員身分打開 Windows PowerShell

    {{< image src="/images/start_first_nodejs/open_powershell.png"  height="200" src_s="start_first_nodejs/open_powershell.png"  src_l="start_first_nodejs/open_powershell.png" alt="open_powershell" caption="Run PowerShell as administrator" linked="false">}}
        
3. 設置權限，選擇可以不需要簽署直接執行本機端的腳本檔案

    ```bash
    Set-ExecutionPolicy RemoteSigned
    ```

    看到 Log 跑出

    ```txt
    PS C:\Windows\system32> Set-ExecutionPolicy RemoteSigned

    執行原則變更
    執行原則有助於防範您不信任的指令碼。如果變更執行原則，可能會使您接觸到 about_Execution_Policies 說明主題 (網址為
    https:/go.microsoft.com/fwlink/?LinkID=135170) 中所述的安全性風險。您要變更執行原則嗎?
    [Y] 是(Y)  [A] 全部皆是(A)  [N] 否(N)  [L] 全部皆否(L)  [S] 暫停(S)  [?] 說明 (預設值為 "N"): 
    ```

    選擇 `y` 同意

    4. 確認權限設置是否成功

        ```bash
        Get-ExecutionPolicy
        ```

        ```txt
        PS C:\Windows\system32> Get-ExecutionPolicy
        RemoteSigned
        ```

- 回到 VSCode，再次輸入

    ```bash
    nodemon index.js
    ```

## 編輯 index.js

```javascript
// use express
const express = require('express')
const app = express()

// use ejs
const path = require('path');
ejs = require('ejs');
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, '/views'))

app.get('/', (req, res) => {
    res.send("HOME PAGE")
})

app.listen('3000', () => {
    console.log("listening port 3000")
})

```

- 讓檔案開始運作

    ```bash
    nodemon index.js
    ```

會在 terminal 看到 listening port 3000


{{< image src="/images/start_first_nodejs/run_indexjs.png"  height="200" src_s="start_first_nodejs/run_indexjs.png"  src_l="run_indexjs/run_indexjs.png" alt="run_indexjs" caption="Run index.js" linked="false">}}
