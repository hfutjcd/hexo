---
title: ubuntu install android sdk ndk
top: false
cover: false
toc: true
mathjax: true
keywords: android sdk
date: 2020-06-8 22:25:43
author: Hunter
img:
password:
summary:
tags:
	[android,sdk]
categories: Android,linux
reprintPolicy:
typora-copy-images-to: ubuntu install android sdk ndk
typora-root-url: 学习android-显示系统
---
# sdk安装
## 安装 android-sdk
```
sudo apt install android-sdk
```

## 下载[cmdline-tool](https://developer.android.com/studio/index.html#downloads)

```
wget https://dl.google.com/android/repository/commandlinetools-linux-6514223_latest.zip
unzip commandlinetools-linux-6514223_latest.zip
```

## 配置环境

  * 在上步解压的zip文件到android-sdk路径"/usr/lib/android-sdk/"新建"mkdir cmdline-tools"，拷贝解压文件到“/usr/lib/android-sdk/cmdline-tools"

  * 修改"~/.bashrc文件，增加以下内容，source ~/ .bashrc文件生效。

  * ```bash
    export ANDROID_SDK_ROOT=/usr/lib/android-sdk
    export PATH=$PATH:$NDK:$ANDROID_SDK_ROOT/cmdline-tools/tools/bin
    ```

## 验证

  * sdkmanager --version 输出版本号
## 查看已安装和可用的平台
```
sdkmanager --sdk_root=/usr/lib/android-sdk --proxy=http --proxy_host=xxx.xxx.com --proxy_port=8080  --list
```
如果不需要代理，可以只用'sdkmanager --sdk_root=/usr/lib/android-sdk  --list'

## 下载编译平台

  ```
  sdkmanager --sdk_root=/usr/lib/android-sdk --proxy=http --proxy_host=xxx.xxx.com .com --proxy_port=8080  "platform-tools" "ndk;18.1.5063045" "platforms;android-29" 
  ```
  
  # 只需要ndk中的adb等工具可以只下载ndk文件，配置一下环境变量。
## 下载&解压

```bash
wget https://dl.google.com/android/repository/android-ndk-r20b-linux-x86_64.zip?hl=zh_cn
unzip android-ndk-r20b-linux-x86_64.zip?hl=zh_cn
```

## 配置环境变量~/.bashrc文件

```bash
export NDK=xxx/android-ndk-r20b
export PATH=$PATH:$NDK
```
  
  
