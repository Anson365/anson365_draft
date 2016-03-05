---
layout: post
title: 如何同时搞定多个女朋友git tool
category: 踩过的坑
tags: git
description: 同pc中多个不同git之间的共存
---

## 背景

  git的作用和地位这里就不在赘诉了。最近笔者用公司电脑维护自己的github（下班时间， ^_^），公司用的自己的gitlab。突然发现在提交公司代码的时候
gitlab上的无法使用。顿时一千万个草泥马奔腾而来，后来发现是新增的github密钥文件将gitlab的给覆盖了，那怎样才能玩转多个git tool，同时搞定
多个女朋友呢？

## 操作

1.首先我们在创建本地git环境的时候，利用ssh-keygen生成密钥的时候要对文件生成文件进行区分。   
    
    如github :   
        github_rsa & github_rsa.pub      
     gitlab :   
        gitlab_rsa & gitlab_rsa.pub    

2.按照要求将XXX_rsa.pub中的内容放入对应的git服务器仓库中        
3.然后进入ssh文件夹找到config，如果没有的话就自己创建一个    
4.按照配置嵌入内容
 
    Host github   
    HostName github.com 
    User anson365
    IdentityFile ~/.ssh/github_rsa
    ...
   
   *根据具体的git进行添加*
好的 大功告成，以后github就是github的账号，其他git就是其他的配置了。

