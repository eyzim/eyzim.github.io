# [Git] 新增專案到 GitHub


1. 在 GitHub 上建立專案 `test`

2. 產生後會在主頁面看到這個提示

3. 回到本機端，走到要上傳的資料夾中

4. 新增 `.git` 紀錄
    ```bash
    git init
    ```

5. 把要記錄的**檔案** ~~作業~~ 加入**暫存區** ~~交作業區~~ (staged)
    - 一一輸入檔案名稱的方式：
        ```bash
        git add index.js
        ```

    - 我沒耐心一次龍齁ㄌㄨㄟˋ的方式：
        ```bash
        git add .
        ```

6. 放入暫存區後，將**暫存區** ~~交作業區~~ 的**檔案** ~~作業~~ 交給 commit ~~小老師~~
    ```
    git commit -m "[commit msg]"
    ```

7. 遠端連線 GitHub 上的 Git
    ```
    git remote add origin https://github.com/eyzim/test.git
    ```

8. 本機端的 Git commit ~~小老師~~ 推到 GitHub ~~老師~~ 那邊的 Git
    ```
    git push --set-upstream origin master
    ```

