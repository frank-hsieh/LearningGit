# Git #
## Description ##
* gittutorial
* giteveryday
* Git User's Manual
## Further Documentation ##
* See **Description** for getting started.
* Git concept chapter of the user-manual 
* gitcore-tutorial
* gitworkflow
    * for an overview of recommended workflows
* ~~howto~~
    * ~~useful examples~~
## See Also ##
* gitturtorial
* gitturtorial-2
* giteveryday
* gitcvs-migration
* gitglossary
* gitcore-tutorial
* gitcli
* The Git User's Manual
* gitworkflow

## 閱讀心得 ##

1. gitworflow 講的是 git project 的 worflow, 先跳過。
2. giteveryday 的 Participant 節，不是很直觀。

## 快速重新閱讀入門文件 (2023/08/28) ##

1. git man page: _See gittutorial[7] to get started_

### gittutorial ###

1. `git commit -a` is equivalent to `git add` plus `git commit`
1. `git log`
1. `git log -p` 這行會顯示 file difference. 我猜是 print.
2. `git branch {name}` create a new branch
3. `git branch` list all branch
4. `git merge {source_branch}`
    1. it might automatially finish the mrege
    2. or, manually solve the conflict
    3. `git commit -a` when done.
5. `git branch -d {name}` delete a branch
6. `git branch -D {name}` 好像是強制刪除的意思

#### collaboration ####
##### 參與者 #####
1. `git clone {source_path} [{dest_path}]`      
##### Owner #####
2. `git pull {source_repo_path} {source_branch_name}`
3. `git fetch {source_repo_path} {source_branch_name}`
    1. `git log -p HEAD..FETCH_HEAD` 檢視我方與對方的差異。FETCH_HEAD儲存對方的tip code.這行會呈現  分岔點到 FETCH_HEAD的修改
    2. `gitk HEAD..FETCH_HEAD` 圖形化顯示歷史
4. `git remote add bob {bob's repo path}` 增加一個remote repository shorthand
    1. `git fetch bob` 對 "bob" remote repo shorthand 執行 fetch，這樣的話，**抓到的東西**會被儲存在bob/master
    2. `git log -p master..bob/master`
    3. `git merge bob/master`
    4. `git pull . remotes/bob/master` 合併的另一做法。注意 pull 永遠只施行於當前branch
        1. 我注意到remote-tracking branch 語法是 `remotes/{remote repo shorthand}/{remote branch name}`

##### 參與者 #####
5. `git pull` 要知道clone別人的人, `git config --get remote.origin.url`會得到被clone方的path
6. `git branch -r`
list remote-tracking branches
7. `git clone {server domain name}:{source_path} {local_repo_name}`
當參與者去別的server上開發，就重新clone 一份 lcoal repo.

#### exploring history ####
1. git show HEAD
2. git show HEAD^
3. git show HEAD^^
4. git show HEAD~4
5. git show HEAD^1
6. git show HEAD^2 ; 這兩行在呈現當HEAD有兩個parents時
7. git tag {tag_name} {SHA1} ; set a tag
8. git branch {branch_name} {tag_name}
9. git reset --hard HEAD^ ; 重置目前的branch 與 working directory 到 HEAD^
10. git grep {string} {object_name} ; 搜尋那一版的全部 source code
11. git log v2.5..v2.6
12. git log v2.5..
13. git log --since="2 weeks ago"
14. git log v2.5.. Makefile ; 只查閱修改 Makefile 的commit
15. git log stable..master ; 可以用分岔想像圖。第一個參數是不走的。第二個參數是有走的。
16. git diff v2.5:Makefile HEAD:Makefile ; 你可以在filename前面加上version
17. git show v2.5:Makefile

#### Next Steps ####
1. 請了解
    1. the object database 用以儲存 files, directories, commits 的資訊
    2. the index file is _a cache of the state of a directory tree_
2. gittutorial-2[7] 為本 tutorial 的 part 2
3. 若不想馬上閱讀 tutorial part 2, 可以看以下主題
    1. git-format-patch, git-am
    2. git-bisect
    3. gitworkflow
    4. giteveryday
    5. gitcvs-migration


