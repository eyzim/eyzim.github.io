# 【Git】初學者筆記


「記得把 code commit 上 Git 啊！」

聽到 Git 的時候，通常腦袋團團轉，到底要怎麼樣才能快速上手呢?

## Learning Git Branching

[Learning Git Branching](https://learngitbranching.js.org/?locale=zh_TW) 是 GitHub 上很有名的專案，用視覺化的練習，讓人不用真的開一個 reposity 也能清楚明瞭 rebase、cherry-pick，玩到後面關卡時，才發現每題都想用 cherry-pick 秒解啊 :smile:

## 小抄

### 最最最基礎

```bash
git add .
git commit -m "一些標籤"
git push
```


### 建立分支

- 新增分支並且移動到那個分支
    ```bash
    git checkout -b <分支的名字>
    ```
- 移動到那個分支
    ```bash
    git checkout <分支的名字>
    ```
- 查看現在分支位置
    ```bash
    git branch
    git push origin newBranch
    ```

### 將版本 merge 到主枝幹

- 在某一個 branch 上
    ```bash
    git rebase master
    ```
    把 branch 的所有紀錄合併到 master 上囉

- 把別的 branch 的 comit 移動成目前的最新 commit
    ```bash
    git branch -f <branch> <commitID>
    ```

![](/images/git_cheetsheet/merge_checkout_rebase.png)

### 復原

- 往上走兩格
    ```bash
    git reset HEAD~2
    ```
    
- 回復到某 id 並新增相同的模樣節點
    ```bash
    git revert commitID
    ```

### 回到上次 commit 的版本

- 重設 local 端的修改，回復至上次 commit 完當下的面貌
    ```bash
    git reset --hard HEAD
    ```
- 回到上n次的版本
    ```bash
    git reset --hard HEAD~n
    ```
- 取消剛剛的 commit，把 commit 收回，回到已經修改過的檔案，但只有 git add 的狀態
git reset --soft HEAD

### 修改記錄檔

- 在現在的 head 後面加上 v3 v5 兩個 commit (不用同分支)
    ```bash
    git cherry-pick v3 v5
    ```
- 開一個 VIM 直接編輯
    ```bash
    git rebase -i HEAD~5 
    ```

## Reference

