---
title: hexo 搭建的坑
top: false
cover: false
toc: true
mathjax: true
keywords: hexo 搭建的坑
date: 2020-03-27 20:18:31
author: hunter
img:
password:
summary:
tags: ["hexo"]
categories: hexo
reprintPolicy:
---
# hexo 搭建/使用中遇到的问题总结
## 问题：github上重定向的域名在hexo clean，hexo g，hexo d之后总是会被情况，需要重新设置。

* 解决方案

>在本地blog的source目录下新建CNAME文件，文件中增加重定向的域名，例如 ‘blog.changdong.ltd’

* 原因

>github重定向会在仓库根目录下创建一个CNAME的文件，内容就是重定向的文件名。在hexo重新生成public文件之后，hexo d操作会删除远程仓库里的CNAME文件，所以重定向就无效了。通过在本地source目录下创建一个一样的CNAME，在hexo g的过程中会在public文件中生成一个CNAME文件，hexo d到远程，就不会导致CNAME文件丢失了。

## 问题：hexo g生成时，报SyntaxError: D:\blog\node_modules\hexo-filter-github-emojis\emojis.json: Unexpected end of JSON input，

* 解决方案

> 方法1： npm uninstall hexo-filter-github-emojis --no-save,然后npm install hexo-filter-github-emojis --save重装
>
> 方法2：  npm update hexo-filter-github-emojis --save   更新。

* 原因

> 可能是json文件损坏了。可以看一下，emojis.json是空的。怀疑是更新还是没有代理或网络，更新失败。