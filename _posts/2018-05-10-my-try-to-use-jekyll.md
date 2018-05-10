---
layout: post
title: 怎样用Ruby+jekyll+GitHub搭建自己的简单博客
description: 使用Ruby，jekyll和GitHub等来搭建自己的简单博客
categories:
 - Ruby
tags: 环境搭建 Ruby Jekyll
---

### 第一步：Ruby和Next主题的安装
1. 从[这里](https://rubyinstaller.org/downloads/)选择Ruby+Devikit的组合文件，然后一路安装就行。
2. 更新`gem`并从下载`bundler`和`Next`主题
```
gem update
```
```
gem install bundler
```
```
    git clone https://github.com/Simpleyyt/jekyll-theme-next.git
    cd jekyll-theme-next
```
可以使用`gem env`来查看我们的`bundler`安装在了哪里    
3. 接下来就是安装依赖
```
bundle update
bundle install
```
4. 运行Jekyll
```
bundle exec jekyll server
```
5. 运行时出现的错误:参考[问题六](http://duxjs.com/2016/11/01/setup-jekyll-on-windows/)的解决，第七个问题可以忽略。

### 第二步：主题等的配置
> 这里主要是参考[Next的使用文档](http://theme-next.simpleyyt.com/getting-started.html)来配置的
> 注意，当我们左侧的导航栏点击的时候没用时，可能是我们将一开始菜单的给注释掉了，所以我们不应该将一开始的英文给注释掉的。