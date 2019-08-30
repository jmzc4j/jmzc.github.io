---
title: Git入门
date: 2019-08-28 17:03:45
tags: git
categories: 学习笔记
---
### what is
- git 是一个免费开源的分布式版本控制工具；
- [书籍-Pro Git](https://git-scm.com/book/en/v2)：有关git的一切可阅读此书获取；

### why use it
- 既可以进行本地版本控制，又可以进行分布式版本控制；
- 不存在集中式版本控制工具的单点故障问题；
- 具有强大的分支管理；
- 由于是Linus开发，天然可以使用Linux命令；

### Git分支
- 假设有三个目录，并做了提交;
  ```
  $ git add README test.rb LICENSE
  $ git commit -m 'The initial commit of my project'
  ```
- Git会对每个子目录进行校验，并将它们作为树对象存储在Git存储库中;然后Git创建一个提交对象，该对象具有元数据和指向根项目树的指针，因此它可以在需要时重新创建该快照。
- ![A commit and its tree](https://git-scm.com/book/en/v2/images/commit-and-tree.png );
- 如果进行一些更改并再次提交，则下一个提交会存储指向紧接其之前的提交的指针。
- ![Commits and their parents](https://git-scm.com/book/en/v2/images/commits-and-parents.png)；
- Git中的一个分支只是一个指向其中一个提交的轻量级可移动指针。Git中的默认分支名称是master。当你开始提交时，你会得到一个master指向你最后一次提交的分支。每次提交时，master分支指针都会自动向前移动。
- ![A branch and its commit history](https://git-scm.com/book/en/v2/images/branch-and-history.png);
- Git中的“主”分支不是特殊分支。它与任何其他分支完全一样。几乎每个存储库都有一个的唯一原因是该git init命令默认创建它，并且大多数人都不愿意更改它。

### how to use it
- 安装

  1. [Linux说明](https://git-scm.com/download/linux);   
  2. [macOS下载地址](https://git-scm.com/download/mac);   
  3. [Windows下载地址]( https://git-scm.com/download/win);             
- 首次配置
  ```
  # 安装Git时应该做的第一件事是设置用户名和电子邮件地址
  # --system：指定系统级别，--global：指定用户级别
  $ git config --global user.name "John Doe"
  $ git config --global user.email johndoe@example.com

  # 使用以下方法查看所有设置及其来源
  $ git config --list --show-origin
  ```
- 获取帮助
  ```
  $ git help <verb>
  $ git <verb> --help
  $ man git-<verb>

  # 如果您不需要完整的联机帮助页面帮助，但只需要快速了解Git命令的可用选项，您可以使用-h或--help,如：
  $ git add -h
   ```
- 常用命令
  1. `$ git init`：在现有目录中初始化存储库；
  1. `$ git add xxx`：将xxx文件从工作区添加到暂存区；untrace-stage；
  1. `$ git commit -m 'initial project version'`：从暂存区永久提交到当前分支；stage-unmodified；
  1. `$ git clone https://github.com/libgit2/libgit2 mylibgit`：克隆存储库到本地并修改存储库的目录名；
  1. `$ git status`：查看文件状态，主要查看文件内容是否发生了变化；
  1. `$ git diff`：查看工作区与暂存区的更改；
  1. `$ git diff --cached`：查看暂存区与上次提交的更改；
  1. `$ git log  [--pretty=oneline]`：查看提交历史，用于回退过去；
  1. `$ git reflog`：查看命令历史，用于追溯未来；
  1. `$ git reset --hard commitID`：切换版本；
  1. `$ git rm --cached README`：删除暂存区文件；
  1. `$ git checkout xxx`：同步到工作区；
  1. `$ git commit --amend`：重做提交；
  1. `$ git remote -v`：查看远程时要使用的短名称存储的URL；
  1. `$ git remote add <shortname> <url>`：为远程库添加一个新的短名称；
  1. `$ git fetch <remote>`：从远处库抓取数据；
  1. `$ git push <remote> <branch>`：推送当前分支到远程库
  1. `$ git remote rename pb paul`：更改远程库别名pb为paul；
  1. `$ git remote remove paul`：删除一个远程库url；
  1. `$ git push <remote>`：抓取并合并到本地；
  1. `$ git branch testing`：创建一个testing分支；
  1. `$ git checkout testing`:切换分支；
  1. `$ git merge hotfix`：合并hotfix分支到当前分支上；
  1. `$ git branch -d hotfix`：删除分支；
  1. `$ git branch -v`：查看每个分支上的最后一次提交；
  1. `$ git branch --merged`：查看已经合并了的分支，不带星号的分支可以删除；
  1. `$ git push <remote> <branch>`：将本地分支推送到一个名称不同的远程分支；
  1. `$ git checkout -b sf origin/serverfix`：设置名称与远程分支不同的本地分支；
  1. `git branch -u origin/serverfix`：更新当前分支为远程分支；
  1. `$ git push origin --delete serverfix`：完成合并删除远程分支；

### 参考文献
- [书籍-Pro Git](https://git-scm.com/book/en/v2);
- [博客园-Git命令总结](https://www.cnblogs.com/chris0710/p/8925977.html);
- [廖雪峰-Git教程](https://www.liaoxuefeng.com/wiki/896043488029600);

### 附件
{% asset_link Java.gitignore gitignore配置 %}