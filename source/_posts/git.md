---
title: Git 使用手冊
date: 2022-12-13 19:15:18
categories: Git
tags: Git
---

<br/>

<div style="padding: 20px; background: #FFF2CB">
部分內容參考自 <a href="https://backlog.com/git-tutorial/tw/" target="_blank">連猴子都能懂的Git入門指南</a>
</div>

## Git
![](git.png "git")
一個分散式版本控制軟體，工程師必備技能

## 基本知識
### commit
將程式改動從暫存區提交到儲存庫

### branch
分支: 將零碎的 commit 整合成一個有順序有意義的組合
一個分支代表一條獨立的開發線

### commit and branch 關係圖
黑點代表 commit，藍線及紅線代表 branch
![commit and branch](commit-and-branch.png "commit and branch")

### merge
使用 merge，可以合併多個歷史記錄。

如下圖所示 bugfix 分支是從 master 分支分開出來的。

![maser 跟 bugfix 分支](maser-bugfix-branch.png "maser-bugfix-branch")

合併 bugfix 分支到 master 分支時， 如果master 分支的狀態是沒有更改過的話，那麼這個合併是非常簡單的。 bugfix 分支的歷史記錄包含了 master 分支的歷史記錄，所以只要把 master 移動到 bugfix 分支就可以導入 bugfix 分支的內容。這樣的合併被稱為 <b>fast-forward（快轉）</b>合併。

![maser merge bugfix (fast-forward)](merge-ff.png "merge-ff")

但是，master 分支的歷史記錄有可能在 bugfix 分支分開後有新的修改。這時候，要把 master 分支的修改內容和 bugfix 分支的修改內容匯合起來。

![maser 跟 bugfix 分支](merge-no-ff-branch.png "merge-no-ff-branch")

匯合兩個修改時會產生一個名為「合併提交」的提交。Master的位置會被更新到新建立的合併提交上。

![maser merge bugfix (non-fast-forward)](merge-no-ff.png "merge-no-ff")

<div style="padding: 1px 20px 20px 20px; background: #FFF2CB">
<div style="margin-left: 0">

![](note.png "note")

</div>

執行合併時，使用 non fast-forward 參數選項 ( git merge -\-no-ff )，即使是可以 fast-forward 的合併也會建立新的提交並合併喔。

![](merge-non-ff.png "merge-non-ff")

執行 non fast-forward 的合併後，分支會維持原狀，要調查在這個分支裡的操作就容易多了。

</div>

---

## 使用情境

### 1. 本地的分支 git 亂掉了，想要直接同步遠端的，怎麼做？
假設分支名稱為 dev
```
git reset origin/dev --hard
```

<br/>

### 2. 想要查看從 Commit A 到 Commit B 的所有程式變動，怎麼做？
```
git reset B --hard
git reset A
```

執行完會看到以下畫面，左邊代表三支檔案變更了，右邊區塊可以看到哪支程式在A/B的變化

![](git-reset-a-b.png "git-reset-a-b")

<div style="color: red">查看完畢後，記得"不要" commit 跟 push 喔</div>
<br>

### 3. 如果要取消某(幾)個 commit 點，怎麼做？

<img style="max-height: 300px" src="/hexo-blog/2022/12/13/git/git-log.png">

假設想取消 C1 跟 C2 的變動, 
有兩種方法:
#### 方法1: 建立新的 commit 點 (建議)

```
git revert C2
git revert C1
```
<img style="max-height: 500px" src="/hexo-blog/2022/12/13/git/git-revert.png">

#### 方法2: 直接把分支 head 移動到 C0
```
git reset C0 --hard
```
-\-hard 參數代表把變動的檔案都清除

<img style="max-height: 300px" src="/hexo-blog/2022/12/13/git/git-reset.png">

<br/>

<div style="color: red">要特別注意斷頭 ( detached head ) 狀況！<br/>如果有其他人也正在使用 main 分支做開發，對於他來說是接著 C3 commit 繼續開發，但是 reset head 到 C0 後，C3 的 commit 就跟 main 分支脫鉤了，導致該開發人員的後續 commit 找不到所屬分支，產生"斷頭"</div>

<br/>

### 4. commit 數量太雜亂，想要合併多個 commit 成為一個，怎麼做？

原本的 commit 如下，想要合併 a1, a2, a3

![](git-rebase-origin.png "git-rebase-origin")


```
git rebase -i c614ead952dfb56f24f754a5f7fb9295232b14da
```
-i 為 interactive ( 互動模式 ) 的縮寫, 開啟編輯頁面 

![](git-rebase-i-default.png "git-rebase-i-default")

修改編輯頁面
1. 把 a2, a3 改成 squash, 代表跟 a1 合併成同一個
2. 按 esc, :wq

![](git-rebase-i-squash.png "git-rebase-i-squash")

再來修改 commit message
1. 修改 message (紅框處)
2. 按 esc, :wq

![](git-rebase-i-commit-message.png "git-rebase-i-commit-message")

將將! 從原本的 3 個 commit 合併成 1 個 commit 了, commit message 也改成 A 囉

![](git-rebase-result.png "git-rebase-result")




---

## 附錄

git 基本操作練習可以使用 [git 遊樂場](https://learngitbranching.js.org/?locale=zh_TW)，或是實際上操作電腦上的 git 指令是學習最快的方式喔