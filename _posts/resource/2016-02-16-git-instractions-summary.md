---
layout: post
title: git常用指令汇总（持续更新）
category: 资料
tags: git
description: git 常用指令及情况汇总
---
git在程序员的日常开发中承担着程序托管、发布等作用。无论是[github](http://github.com)、[gitlab](https://about.gitlab.com/)、[git@osc](http://git.oschina.net/)其中运用到的git指令都是一样的。
文章是笔者学习工作中使用的git情况和解决方案，旨在总结方便查阅，同时加深对git的灵活运用，因此文章的内容会进行持续更新。当然对于未提及到或者有更优越的解决方案的情况，也欢迎大家留言探讨。
##clone 
1.普通clone（clone所有分支）
> git clone <repo>

2.仅clone指定分支
> git clone -b <branch_name> <repo>

##branch
1.查看分支
> 查看本地分支：git branch   
> 查看远程分支：git branch -r

2.切换分支
> git checkout <branch_name>

3.创建分支
> git branch <branch_name>

4.删除分支
> git branch -d <branch_name>

5.合并分支
> git branch merge <branch_name_need_merge_together>

##常用功能

###内容比较
> 版本间比较：git diff <version1> <versiong2>
> 本地与版本比较：git diff <version>

###提交信息及日志
> git log

###内容还原
>还原到指定版本：git reset <version>
>指定文件还原到最新版本(内容回滚)：git checkout <file_name>

