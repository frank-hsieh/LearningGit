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

```
git add file1 file2 file3
```
Add multiple files

```
git diff            # 呈現修改了『但是』還沒有加入index的
git diff --cached   # 呈現已經被 git add 的內容
```


