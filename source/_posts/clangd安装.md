---
title: clangd安装
top: false
cover: false
toc: true
mathjax: true
keywords: bat
date: 2021-03-24
author: Hunter
img:
password:
summary:
tags:
	[clangd]
categories: 编译工具
reprintPolicy:
typora-copy-images-to: 
typora-root-url: clangd安装
---
# ubuntu 安装 clangd

## 正常通过apt-get安装即可
```shell
sudo apt-get install clangd
```
顺利的话，通过以上命令即可安装。
## 可能的问题
### 找不到源
通常情况下如果你没有配置过专门的apt.source.list源的话，会报找不到安装包的问题。这时候需要配置源文件，
cat /etc/apt/apt.source.list
以ubuntu 16.04为例添加如下命令：(不同版本源不同，参考https://apt.llvm.org/)
```shell
deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial main
deb-src http://apt.llvm.org/xenial/ llvm-toolchain-xenial main
```
### 找不到签名
添加源后，执行apt-get update可能汇报签名错误，如下
```shell
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 15CF4D18AF4F7421
```
需要继续添加签名文件
以old-stable 分支为例，执行
```shell
wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -
```
执行成功之后再次执行
```shell
sudo apt-get update
sudo apt-get install clangd
```
不出意外的话，应该可以安装成功。
