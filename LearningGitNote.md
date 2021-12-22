start from here https://git-scm.com/docs/git?
  
git tutorial https://git-scm.com/docs/gittutorial

complete manual https://git-scm.com/docs/user-manual

git everyday https://git-scm.com/docs/giteveryday

git 2.34.1 推荐閱讀 gittutorial-2

git-svn https://git-scm.com/docs/git-svn

# gittutorial 筆記
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
```
git pull . remotes/bob/master
```
這個指令有什麼不同? 文件說這個等效上一個指令。(1) pull 就是 fetch + merge (2) 新見到一個字：remotes，這是否代表 remote-tracking branches 的集合呢? 注意 pull 總是拉到current branch.

```
bob$ git pull
```
因為Bob 是clone Alice's repository, 所以git pull 對他有效。
```
git config --get remote.origin.url
git config -l                       # show complete configuration
```
如果你是clone 別人的repository, 第一個指令可以讓你看到clone的source.
```
bob$ git branch -r
```
在Bob 端，Git 似乎會保留所有的Alice的branch. 這些branch都是```origin/```開頭.

心得：若我是clone者，我的源頭會被稱為```origin/```. 若我使用```remote add```把別人加成我的一個shorthand，看起來會被稱為```remotes/shorthand_name```

```
git clone alice.org:/home/alice/project myrepo
```
跨機器的clone語法。

## 關於History
```
git show c82a22
git show HEAD
git show experimental
git show HEAD^
git show HEAD^^
git show HEAD~4       # 等效 git show HEAD^^^^, 你可想成是-4
```
```
git show HEAD^1         # the first parent
git show HEAD^2         # the second parent
```
```
git tag v2.5 1b2e1d
git diff v2.5 HEAD
git branch stable v2.5 # 從v2.5這個commit創建branch stable
git reset --hard HEAD^ # 當前branch的頭與working directory都會被設到 HEAD^
```
注意! 最後的指令會清除任何local changes。如果你下了這指令，也會強迫所有從你這裡pull的人多做merge來使history消失。
注意! 如果只是要撤消曾經push過的commit, 用git revert就好。

```
git grep "hello" v2.5 # 好像是search 單一commit
git grep "hello"      # 搜尋所有Git管理的files
```
```
git log v2.5..v2.6
git log v2.5..
git log --since="2 weeks ago"
git log v2.5.. Makefile
git log stable..master
```
```
gitk --since="2 weeks ago" drivers/
```
以git管理的repository常常有merge。所以以list呈現的git log不太有意義。gitk是比較好的呈現。

```
git diff v2.5:Makefile HEAD:Makefile.in
```
比較兩個不同commit且不同檔名的的檔案
```
git show v2.5:Makefile   # 閱讀內容
```
# gitworkflows 筆記
```
git rebase --interactive  # 好像可以edit commit
```
```
git stash push --keep-index # 好像可以先把local change收起來, 參見 git-stash
```
兩大merge工具是 git merge 與 git cheery-pick
前者作用於branch leve; 後者作用於commit level。git document比較推荐 git merge.
## 先暫停。此主題暫時不深入學習。

# giteveryday 筆記

## 單一開發者
* git init
* git log
```
git log --since='3 days ago'
git log -10                  # 只看最近10次commit
git log --until 2005-12-10   # 從最舊直到2005/12/10, 由新到舊列出
git log v2.43.. curses/
```
* git switch; git branch
```
git swtich -c alsa-audio # create a new topic branch
```
* git add
* git diff; git status
```
git diff HEAD^ # 這方法可以看到你未commit的修改與上一次commit的修改
```
* git commit
```
git commit -a -s # with your sign-off
git commit -a --amend     # 修改最近一次的commit, 合入目前working directory的修改，保持message不變
```
* git restore
```
git restore curses/ux_audio_oss.c # 捨棄本地修改
```
* git merge
* git rebase (不熟悉)
* git tag
## 單一開發者(參與者)
* git clone
* git pull; git fetch
```
git pull    # 如果你的master 是不含自己修改的，在master 用git pull挺不錯的
git log -p ORIG_HEAD.. arch/i386/include/asm-i386 # 需了解ORIG_HEAD
git ls-remote --heads http://git/kernel.org/.../jgarzik/libata-dev.git # 列出某個external repository的所有branches
git pull git://git.kernel.org/pub/.../jgarzik/libata-dev.git ALL # 從特定repository的特定branch "ALL" fetch and merge
git reset --hard ORIG_HEAD # 完全丟棄這一次的pull (ORIG_HEAD大概是上上一次pull的HEAD)
git gc    # 做過git reset --hard, 做這件事蠻合理的
```
* git push
* git format patch
```
git format-patch master # extract patches from your branch, releative to master.
```
* git send email
```
git send-email --to="person <email@example.com" 00*.patch
```
* git request pull
### push into another repository (postpone the study)
## 整合者
* git am       # apply patched e-mailed
* git pull
* git format patch
* git revert
* git push    # 推送最新鮮的commits
## 程式庫管理者 (postpone the study)
* git daemon
* git shell
* git http backed 
* git web




