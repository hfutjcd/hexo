---
title:bat脚本不弹窗执行
top: false
cover: false
toc: true
mathjax: true
keywords: bat
date: 2021-01-25
author: Hunter
img:
password:
summary:
tags:
	[bat]
categories: 编程
reprintPolicy:
typora-copy-images-to: 
typora-root-url: bat脚本不弹窗执行
---
目的：
  通过脚本执行一个长期服务的时候，不希望服务一直在前台运行，在点击脚本后，自动切到后台。
  以adbforward为例。
  
 ```shell
 @echo off

if "%1"=="h" goto begin

start mshta vbscript:createobject("wscript.shell").run("""%~nx0"" h",0)(window.close)&&exit

:begin

::以下为正常批处理命令，不可含有pause set/p等交互命令
java -jar E:\tools\adbforward\windows\adb-forward-windows\adbportforward.jar server adblocation=E:\tools\adbforward\windows
pause

 ```
