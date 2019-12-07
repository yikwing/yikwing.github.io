# git 常用命令


Git 删除暂存区或版本库中的文件

<!--more-->

## 1. 基础

我们知道 Git 有三大区（工作区、暂存区、版本库）以及几个状态（`untracked`、`unstaged`、`uncommited`）。

1. 打开你的项目文件夹，除了隐藏的.git 文件夹，其他项目文件位于的地方便是工作区，工作区的文件需要添加到 Git 的暂存区（git add），随后再提交到 Git 的版本库（git commit）。

2. 首次新建的文件都是 untracked 状态（未跟踪），此时需要 git add 到暂存区，Git 便会在暂存区中生成一个该文件的索引，文件此时处于 uncommited 状态，需要 git commit 生成版本库。添加到了版本库之后，再对文件进行修改，那么文件的状态会变为 unstaged 状态。

## 2. 删除错误添加到暂存区的文件

1. 仅仅删除暂存区里的文件

```git
git rm --cache 文件名
```

2. 删除暂存区和工作区的文件

```git
git rm -f 文件名
```

## 3. 删除错误提交的 commit

```git
//仅仅只是撤销已提交的版本库，不会修改暂存区和工作区
git reset --soft 版本库ID


//仅仅只是撤销已提交的版本库和暂存区，不会修改工作区
git reset --mixed 版本库ID


//彻底将工作区、暂存区和版本库记录恢复到指定的版本库
git reset --hard 版本库ID
```
