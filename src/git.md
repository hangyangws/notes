# Git 只言片语

[link](http://yanhaijing.com/git/2017/01/19/deep-git-0/)  
[link](http://gitref.org/zh/basic)  
[link](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

### git clone

`git clone url` 检出代码到本地，以项目名新建文件夹  
`git clone <url> [FolderName]` 自定义文件夹名「默认为项目名字」

### git add

添加文件到缓存区，可以接多个文件名，以空格分开  
`git add .`：添加所有修改  
`git add *`：添加当前目录所有修改

### git diff

退出快捷键：`q`  
`git diff` 列出未上次提交快照之后尚未缓存的所有更改  
`git diff --cached` 已经写入缓存的改动  
`git diff HEAD` 显示 **所有** 的改动

### git status

查看当前修改状态，`git status -s` 为输入简洁版  
文件左边为红色表示没有追踪，绿色反之

### git commit

将 `add` 的缓存文件记录快照  
`git commit --amend`：修改提交信息  
`git commit --amend --author='name <email>'`：修改上一次提交的作者信息  
`git commit -a` 对所有有提交记录的文件执行`git add`，所以不会执行新增文件  
提交已经追踪的代码（status 左边为绿色的）

### git reset HEAD

取消缓存已缓存的内容  
`git reset HEAD -- <fileName>` 取消默认文件的缓存

### git checkout

`git checkout <branchName>` 切换分支  
`git checkout -b <branchName>` 先创建分支，再切换到该分支

### git branch

`git branch` 列出本地分支  
`git branch -r` 列出远端分支  
`git branch -a` 列出所有分支  
`git branch -d <branchName>` 删除本地分支『前提是目前分支不在此分支』
`git branch -m old new` 修改分支名

### git log

`git log` 显示一个分支中提交的更改记录  
`git log --oneline` 选项来查看历史记录的紧凑简洁的版本  
`git log --graph` 选项来查看历史记录的拓扑图  
添加 `-5` 查看 5 条记录  
提交 `--author=userName` 只查看某个人的提交

### git remote

`git remote` 列出远端别名  
`git remote -v` 列出远端别名，并且可以看到每个别名的实际链接地址

### git reset

git reset --hard HEAD~30

### git pull

pull 和 fetch 区别

### 新建分支

1. `git branch branchName`
1. `git checkout branchName`

OR

1. `git checkout -b branchName`

### 多个 commit 合并为一个

1. `git rebase -i origin/master`
1. `git push origin branchName:branchName -f`

### 当前分支合并 master 新的代码

1. `git pull --rebase origin master`
1. `解决冲突` 「用编辑器把冲突的代码改好」
1. `git add .` 「添加相应的修改文件」
1. `git rebase --continue`
1. `git push origin branchName:branchName -f`

OR

1. `git fetch origin master`
1. `git rebase origin/master`
1. `解决冲突` 「用编辑器把冲突的代码改好」
1. `git add .` 「添加相应的修改文件」
1. `git rebase --continue`
1. `git push origin branchName:branchName -f`

### 删除远程分支

`git push origin :branchName`

### 清理远程分支，把本地不存在的远程分支删除

`git remote prune origin`

### 修改当前项目的配置信息

`git config user.name 'Name'`  
`git config user.email 'Name@xx.com'`

### reset --soft

`git reset --soft 495f7482730bcc110308faa1258432ef5cef8cda`:  「后面这一串为 hash 值，可以用过 `git log` 查看」
回到某一次提交

[link](https://github.com/geeeeeeeeek/git-recipes/wiki/2.6-%E5%9B%9E%E6%BB%9A%E9%94%99%E8%AF%AF%E7%9A%84%E4%BF%AE%E6%94%B9#git-reset)
[link](http://www.cnblogs.com/kidsitcn/p/4513297.html)

### git stash

`git stash`：把当前的修改「藏起来」  
`git stash pop`：把「藏起来」的修改取出来

### 修改远程仓库地址

1. `git remote set-url origin [url]`
1. `git remote rm origin` && `git remote add origin [url]`
1. 直接修改 config 文件

### git config core.ignorecase false

让 git 对文件大小写敏感

### gco --theirs '冲突文件名'

解决冲突使用别人的代码

### 删除远端所有非 master 的分支

`git branch -a | grep remotes/lcj | grep -oP "remotes/lcj/\K.*" | xargs -n 1 -i git push lcj :{}`

