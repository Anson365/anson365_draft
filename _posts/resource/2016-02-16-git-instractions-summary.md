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
> git clone &lt;repo

2.仅clone指定分支
> git clone -b &lt;branch_name&gt; &lt;repo&gt;

##branch
1.查看分支
> 查看本地分支：git branch   
> 查看远程分支：git branch -r

2.切换分支
> git checkout &lt;branch_name&gt;

3.创建分支
> git branch &lt;branch_name&gt;

4.删除分支
> git branch -d &lt;branch_name>

5.合并分支
> git branch merge &lt;branch_name_need_merge_together&gt;

##常用功能

###内容比较
版本间比较：
> git diff &lt;version1> &lt;versiong2&gt;

本地与版本比较：
> git diff &lt;version&gt;

###提交信息及日志
> git log

###内容还原
还原到指定版本：
>git reset &lt;version&gt;

指定文件还原到最新版本(内容回滚)：
>git checkout -- &lt;file_name&gt;

###撤销添加
撤销add：    
> git reset HEAD &lt;file_name&gt;    

撤销commit：
>    git reset --hard &lt;commit_id&gt;
>    git push origin HEAD --force
