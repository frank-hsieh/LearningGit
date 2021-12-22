start from here https://git-scm.com/docs/git?
  
git tutorial https://git-scm.com/docs/gittutorial

complete manual https://git-scm.com/docs/user-manual

git everyday https://git-scm.com/docs/giteveryday

# 正文
```
git config --global user.name "Frank Hsieh"
git config --global user.email klhsieh@kunlung.com
```  
儲存自己的資訊

```
git init
```
convert one directory into git control

```
git add .
```
Add everything (in the direcotry) into git.
做了這件事, 這個snapshot 就被存入一個暫時的 staging area, Git 叫它 "index"
git commit之後，資訊會永遠存在 repository內。

```
git add file1 file2 file3
```
Add multiple files

```
git diff            # 呈現修改了『但是』還沒有加入index的
git diff --cached   # 呈現已經被 git add 的內容
```
```
git status
```
如果你又做了新的修改，你必須要再git add一次。
否則你commit的只有你的"第一期"修改。
```
git commit -a
```
這代表「把所有修改的files都加入index」
git add 可以用在「加新檔案」或是「加新修改」
index代表「好幾期你用git add加入的修改」

```
git log         # 只看log messages
git log -p      # 看log 與 修改的內容
git log --stat --summary # 看log message, 但會顯示修改的檔案與增/刪行數
```
```
git branch new_branch_name # create a new branch
git branch                 # list all branch
git switch target_branch   # switch to a certain branch
git branch -d one_branch   # delete a branch, 好像會確認其內容已經merge過來
git branch -D one_branch   # delete a branch, 好像不會確認已經merge了
```
```
git merge another_branch        # merge anotther_branch到current branch內
```

## 與其他人合作
例：Alice 先開始一個project, Bob之後加入

```
git clone /home/alice/project myrepo        # Bob clone Alice's project
```
Bob `git commit -a` 的結果只會在他自己的history內。他可以要Alice pull 他的changes.
```
git pull /home/bob/myrepo master
```
語法是 git pull colleagues_working_copy colleagues_branch_name

pull 其實包含了兩個動作：fetch and merge. Alice 其實也可以只做fetch，這樣就可以不馬上解決衝突問題。
```
git fetch /home/bob/myrepo master
git log -p HEAD..FETCH_HEAD
gitk HEAD..FETCH_HEAD               # 圖形化表示
```
fetch 完成時，bob的changes就會存在Alice的某個東西中。
HEAD..FETCH_HEAD 這語法表示任何可以達到FETCH_HEAD的版本但不包含可以到達HEAD的版本。
(換言之就是從分岔點直到FETCH_HEAD)
我的猜測：FETCH_HEAD這個應該是每次執行過 fetch command之後會更新。

```
gitk HEAD...FETCH_HEAD
```
這語法表示「想看Alice自分岔點後的修改與Bob自分岔點後的修改」。

Alice 可以使用這種技巧：
* stash work-in-progress
* pull
* unstash work-in-progress

```
git remote add bob /home/bob/myrepo
```
語法是 git remote add remote_repository_shorthand remote_repository_path
之後就可以
```
git fetch bob
```
注意喔，這可是會儲存在Alice這邊的(用這種 remote repository shorthand)。它其實是儲存在一個叫 remote-tracking branch裡面。(這個remote tracking branch其實真名叫 bob/master)
```
git log -p master..bob/master
```
這也是看到Bob自分岔點後的修改。
```
git merge bob/master
```
Alice 用這個指令來merge Bob的修改。


