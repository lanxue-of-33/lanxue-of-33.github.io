---
layout: post
title: SSH连接GitHub
description: 这里是如何配置ssh，并连接GitHub，使用Git来提交的自己本地项目的初始教程
categories:
 - Git
tags: Git SSH
---
> 虽然这一步很简单，但终究是入门的要求，不管怎样还是记录一下吧

## 第一步：创建本地资源库或从GitHub上克隆项目
[参考链接](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743256916071d599b3aed534aaab22a0db6c4e07fd0000)
1. 创建空的项目并初始化
```
    $ mkdir test && cd test;
    $ git init
```
2. 新建一个文本并将其添加到资源库中
```
    $ git add readme.txt
    /*执行上一句没有反应就代表成功了,意思是把文件添加到仓库*/
    $ git commit -m "wrote a readme file"
    /*代表将文件提交到仓库，-m后接上的是我们这次提交的说明，可以多次add，一次commit*/
```

## 第二步：ssh链接并将内容推送到GitHub上
1. 查看是已经存在ssh密钥（有`id_rsa.pub`这个文件则代表存在)   
    打开git bash,输入
```
    $ cd ~/.ssh
    $ ls
```
2. 如果没有的话，就自己如下生成ssh密钥；存在的话就直接执行第三步。
```
    $ ssh-keygen -t rsa -C "your_email@example.com"
```
代码参数含义：
* -t 指定密钥类型，默认是rsa，可以省略
* -C 设置注释文字，比如邮箱
* -f 指定密钥文件存储文件名  
可以直接一路回车，不要密码     
3. 查看自己的公钥
```
    $ cat ~/.ssh/id_rsa_pub
```
然后将所出现的结果复制（这就是我们所需要的公钥了）  
4. 登录GitHub账户，点开`your profile`，点击`edit profile`,`SSH and GPG Keys`,`New SSH key`，(如果有存在了的可以全删了)。
5. 将复制到的公钥内容粘贴进`key`域中。`title`域自己随便起个名字，然后点击`Add key`。
6. 验证能不能工作：
```
    $ ssh -T git@github.com
```
如果看到：
> Hi xxx! You've successfully authenticated, but GitHub does not # provide shell access.
则代表你的设置已经成功了。
7. 进入本地库并让本地库关联我们的远程库
```
    $ git remote add origin git@github.com:your-github-name/your-reo-name.git
```
说明:    
* 添加之后远程库的名字就是`origin`,这是git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。
8. 将本地库的内容推送到远程库上
```
    $ git push -u origin master
```
说明:   
* 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，这时git不但会把本地的`master`分支
内容推送到远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或
拉取时就可以简化命令（不用再加`-u`参数）了。
9. 参考链接    
[百度参考一](https://www.cnblogs.com/yzg1/p/5773362.html)
[百度参考二](https://www.cnblogs.com/superGG1990/p/6844952.html)
