---
layout: post
title: "Git命令快速参考"
date: 2013-03-23 14:54
comments: true
categories: ["git"]
---

安装和初始化
----------------

*	配置全局用户名和电子邮件地址

		$ git config --global user.name "Your Name"
		$ git config --global user.email "you@example.com"

*	为特定的版本库配置用户名和电子邮件地址

		$ cd /path/to/repo
		$ git config user.name "Your name"
		$ git config user.email "you@example.com"

*	在命令行中使用不同颜色显示不同内容

		$ git config --global color.ui "auto"

*	初始化版本库

		$ mkdir /path/to/repo
		$ cd /path/to/repo
		$ git init
		Initialized empty Git repository in /path/to/repo/.git/
		$
		... create file(s) for first commit ...
		$ git add .
		$ git commit -m 'initial import'
		Create initial commit bdebe5c: initial import
		 1 files changed, 1 insertions(+), 0 deletions(-)
		 create mode 100644 <some file>

*	克隆版本库

		$ git clone <repository url>
		Initialize repo/.git
		Initialize empty Git repository in /work/<remote repository>/.git/

*	将目录中的内容纳入Git版本控制

		$ cd /path/to/existing/directory
		$ git init
		Initialized empty Git repository in /path/to/existing/directory/.git/
		$ git add .
		$ git commit -m "initial import of some project"

*	在本地版本库中设置远程版本库的别名

		... from within the repository directory ...
		$ git remote add <remote repository> <repository url>

日常操作
------------

*	添加新文件或暂存已有文件上的改动，然后提交

		$ git add <some file>
		$ git commit -m "<some message>"

*	暂存已有文件上的部分修改

		$ git add -p [<some file> [<some file> [and so on]]]

*	使用交互方式添加文件

		$ git add -i

*	暂存已纳入Git版本控制之下的文件的修改

		$ git add -u [<some path> [<some path>]]

*	提交已纳入Git版本控制之下的文件的所有修改

		$ git commit -m "<some message>" -a

*	清除工作目录树中的修改

		$ git checkout HEAD <some file> [<some file>]

*	取消已暂存但尚未提交的修改的暂存标识

		$ git reset HEAD <some file> [<some file>]

*	修复上一次提交中的问题

	*改动相关文件，并暂存*
		
		$ git commit -m "<some message>" --amend

*	修复上一次提交中的问题，并复用上次的提交注释

		$ git commit -C HEAD --amend

分支
----

*	列出本地分支

		$ git branch

*	列出远程分支

		$ git branch -r

*	列出所有分支

		$ git branch -a

*	基于当前分支（的末梢）创建新分支

		$ git branch <new branch>

*	检出另一条分支

		$ git checkout <some branch>

*	基于当前分支创建新分支，同时检出该分支

		$ git checkout -b <new branch>

*	基于另一个起点，创建新分支

	*你可以从版本库中任何一个版本开始创建新分支。这个起始点可以用一条已有的分支名称、一个提交名称，或者一个标签名称来表达。*

		$ git branch <new branch> <start point>

*	创建同名新分支，覆盖已有分支

		$ git branch -f <some existing branch> [<start point>]

*	移动或重命名分支

	*只有当\<new branch\>不存在时*

		$ git checkout -m <existing branch name> <new branch name>

	*如果\<new branch\>已存在，就覆盖它*

		$ git checkout -M <existing branch name> <new branch name>

*	把另一条分支合并到当前分支

		$ git merge <some branch>

*	合并，但不提交

		$ git merge --no-commit <some branch>

*	拣选合并，并且提交

		$ git cherry-pick <commit name>

*	拣选合并，但不提交

		$ git cherry-pick -n <commit name>

*	把一条分支上的内容压合到另一条分支（上的一个提交）

		$ git merge --squash <some branch>

*	删除分支

	*仅当欲删除的分支已合并到当前分支时*

		$ git branch -d <branch to delete>

	*不论欲删除的分支是否已合并到当前分支*

		$ git branch -D <branch to delete>

撤销变更
--------

*	反转某个提交

		$ git revert <commit>

*	取消已暂存但尚未提交的变更的暂存标识

		$ git reset HEAD <some file>

*	删除工作目录树中的变更

		$ git checkout <some file>
	
	**# 警告！此操作本身不可撤销！**

*	在版本库中撤销已提交的变更，并在工作目录树中清除

		$ git reset --hard HEAD^

	**# 警告！此操作本身不可撤销！**

历史
----

*	显示全部历史记录

		$ git log

*	显示版本历史，以及版本间的内容差异

		$ git log -p

*	只显示最近一个提交

		$ git log -1

*	显示最近的20个提交，以及版本间的内容差异

		$ git log -20 -p

*	显示最近6小时的提交

		$ git log --since="6 hours"

*	显示两天之前的提交

		$ git log --before="2 days"

*	显示比HEAD（当前检测出分支的末梢）早3个提交的那个提交

		$ git log -1 HEAD~3

	或者

		$ git log -1 HEAD^^^

	或者

		$ git log -1 HEAD~1^^

*	显示两个版本之间的提交

	*下面命令中的<start point>和<end point>可以是一个提交名称、分支名称、标签名称，或者它们的混合。*

		$ git log <start point>...<end point>

*	显示历史，每个提交显示一行，包括提交注释的第一行

		$ git log --pretty=oneline

*	显示改动行数统计

		$ git log --stat

*	显示改动文件的名称和状态

		$ git log --name-status

*	显示当前工作目录树和暂存区间的差别

		$ git diff

*	显示暂存区和版本库间的差别

		$ git diff --cached

*	显示工作目录树和版本库间的差别

		$ git diff HEAD

*	显示工作目录树与版本库中某次提交版本之间的差别

		$ git diff <start point>

*	显示版本库中两个版本之间的差别

		$ git diff <start point> <end point>

*	显示差别的相关统计

		$ git diff --stat <start point> [<end point>]

*	显示文件中各个部分的修改者及相关提交信息

		$ git blame <some file>

	*显示文件中各个部分的修改者及相关提交信息，包括在该文件中复制、粘贴和移动内容等方面的情况。*

		$ git blame -M <some file>

*	显示文件中各部分的修改者及相关提交信息，包括在文件移动内容方面的情况

		$ git blame -C -C <some file>

*	显示历史时，显示复制和粘贴信息

		$ git log -C -C -p -1 <some point>

远程版本库
----------

*	克隆远程版本库

		$ git clone <some repository>

*	克隆远程版本库，但只下载其中最近200个提交的历史记录

		$ git clone --depth 200 <some repository>

*	在本地版本库中设置远程版本库的别名

		$ git remote add <remote repository> <repository url>

*	显示远程分支

		$ git branch -r

*	基于远程分支创建本地分支

		$ git branch <new branch> <remote branch>

*	基于远程标签创建本地分支

		$ git branch <new branch> <remote tag>

*	从别名“origin”的远程版本库中取来修改变化，但不合并到本地分支

		$ git fetch

*	从任意的远程版本库中取来修改变化，但不合并到本地分支

		$ git fetch <remote repository>

*	从任意的远程版本库中取来修改变化，并合并到当前检出的本地分支

		$ git pull <remote repository>

*	从别名为“origin”的远程版本库中取来修改变化，并合并到当前检出的本地分支

		$ git pull

*	把修改变化从本地分支推入远程版本库

		$ git push <remote repository> <local branch>:<remote branch>

*	把修改变化从本地分支推入远程版本库中同名分支

		$ git push <remote repository> <local branch>

*	把修改变化从本地新建分支推入远程版本库

		$ git push <remote repository> <local branch>

*	把修改变化推入别名为“origin”的远程版本库

	*当远程版本库中已有同名分支时，这个命令会推入本地分支到远程版本库对应的分支中。如果远程版本库中尚无同名分支，则须使用 git push <repository name> <local branch>*

		$ git push

*	在远程版本库中删除分支

		$ git push <remote repository> :<remote branch>

*	在本地版本库中删除所有远程版本库中已不存在的分支

		$ git remote prune <remote repository>

*	在本地版本库中删除某个远程版本库的简称，以及该远程版本库相关的分支

		$ git remote rm <remote repository>

连接Git和SVN
------------

*	克隆SVN版本库的全部内容

		$ git svn clone <svn repository>

*	克隆具有标准结构的SVN版本库

	*下面命令适用于克隆标准结构的SVN数据库，也就是说，主干命名为trunk，其他分支都存放于branches目录下，标签都存放于tags目录下。*

		$ git svn clone -s <svn repository>

*	克隆非标准结构的SVN版本库

		$ git svn clone -T <trunk path> -b <branch path> -t <tag path> <svn repository>

*	克隆具有标准结构的SVN版本库中的某个版本（比如第2321版）

		$ git svn clone -s -r 2321

*	克隆具有标准结构的SVN版本库，并对SVN中的分支添加前缀

		$ git svn clone -s --prefix svn/ <svn repository>

*	从上游SVN版本库中获得更新的内容，并依此在本地Git版本库中变基本地分支

		$ git svn rebase

*	把修改变化推回上游SVN版本库

		$ git svn dcommit -n

*	显示SVN历史记录

		$ git svn log

*	显示文件中各个部分的SVN的修改者及相关提交信息

		$ git svn blame <some file>

参考资料
--------

* [Pragmatic Version Control Using Git](http://pragprog.com/book/tsgit/pragmatic-version-control-using-git)（以上都是来自本书）
* [Git Doc](http://git-scm.com/docs)
* [Github Help](https://help.github.com/)
