---
title: oh-my-zsh终端
top: false
cover: false
toc: true
mathjax: true
keywords: zsh
date: 2021-03-18
author: Hunter
img:
password:
summary:
tags:
	[shell]
categories: linux
reprintPolicy:
typora-copy-images-to: 
typora-root-url: oh-my-zsh终端
---
*主要转自：https://www.itshutong.com/articles/281/oh-my-zsh-the-best-shell-none，有部分修改*

命令行是程序员的最爱，默认的 bash shell 虽然功能已经很强大，但显得太朴素了，也不够智能，远远谈不上酷炫，虽然已经远远将 windows 的 cmd 甩开几百条街。但对于极客来说，bash 还是太弱了，长得也难看。

`zsh` 的功能比bash强大很多，但配置过于复杂，起初只有极客才在用。后来，有个穷极无聊的程序员可能是实在看不下去广大猿友一直只能使用单调的bash, 于是他创建了一个名为 `oh-my-zsh` 的开源项目...

自此，只需要简单的安装配置，小白程序员们都可以用上高档大气上档次，狂拽炫酷吊炸天的 [oh-my-zsh](https://ohmyz.sh/)

![img](https://image-1253761983.picgz.myqcloud.com/2019-08-18-124800.png)

## 安装zsh

以centos7为例：(mac也是类似的操作)

查看系统是否安装了zsh，如果没找到 `/bin/zsh` ，就需要先安装

```bash
$ cat /etc/shells 
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
/bin/tcsh
/bin/csh
```

Bash

> centos7默认情况下没安装zsh

用 `yum` 安装 `zsh`

```bash
$ sudo yum -y install zsh
```

Bash

安装成功

```bash
$ cat /etc/shells | grep zsh
/bin/zsh
```

Bash

切换shell为zsh

```bash
$ chsh -s /bin/zsh
Changing shell for root.
Shell changed.
```

Bash

退出，重启终端，可看到shell已切换成功

```bash
$ echo $SHELL 
/bin/zsh
```

Bash

## 安装 oh my zsh

一行命令即可自动化安装

```bash
$ wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

Bash

成功界面：

![img](https://image-1253761983.picgz.myqcloud.com/2019-06-14-031820.png)

退出重新登入终端，即可看到oh-my-zsh已经生效

接下来只需简单的配置，就可以享受 `oh-my-zsh` 的强大

## 主题选择

oh-my-zsh有很多漂亮的主题:

在 [主题列表](https://github.com/robbyrussell/oh-my-zsh/wiki/themes) 中选择自己心宜的主题，安装很简单，打开配置文件 `~/.zhsrc` ，将 `ZSH_THEME` 值改为你所选的主题名称
主题文件在".oh-my-zsh/themes"文件夹下，可自行查看支持的主题。
如：

```bash
$ vim ~/.zshrc
# 修改主题名称
ZSH_THEME="cloud"
```

然后重新加载配置文件

```bash
$ source ~/.zshrc
```

> 每次修改配置文件后，都需要再次加载配置文件才能生效

## 插件

oh-my-zsh默认安装了git插件，在git仓库中，会提示当前所在的分支。此外，还有大量优秀的插件，具体可查看 [插件仓库](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins)，以下只介绍几个我常用的插件

- extract

linux下的压缩文件有多种格式，我总是忘记不同格式的解压命令，如果有一个通用的指令可以解压所有文件就好了，[extract](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/extract) 插件就能解决此问题

现在不管是 `zip, tar.bz2, gz` 等格式的压缩文件，都只需要执行 `extract 文件名` 即可

- z

[z](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/z) 插件可以智能在历史的cd命令中智能地选择指令，如，我们在终端执行过`cd /usr/local/nginx/conf/vhost`，以后只需要执行`z vhost`即可，不必再去敲长串的命令了

> 以上两个插件都是`oh-my-zsh`自带的，只需要在配置文件的plugins配置中加入插件名称即可，多个插件用空格隔开

- zsh-autosuggestions

[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) 属于第三方插件，可以提示我们插入历史命令

如，我执行过 `php artisan ide-helper:generate` 指令，下次当我输入 `php` 时，就会自动提示指令，并且会随着我们的输入而实时地匹配最符合的历史指令，当所提示的指令符合我们的预期，只需要按右箭头即可补全

![img](https://image-1253761983.picgz.myqcloud.com/2019-06-14-035331.png)

[安装此插件的流程](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)：

```bash
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Bash

在配置文件中中添加插件名称

综上，配置文件plugins的内容如下：

```bash
plugins=(
  git
  extract
  z
  zsh-autosuggestions
)
```

Bash

重新载入配置文件，即可生效

## 卸载 Oh My Zsh
终端输入 ：
```bash
uninstall_oh_my_zsh
Are you sure you want to remove Oh My Zsh? [y/N]  Y
```
终端提示信息：
```bash
Removing ~/.oh-my-zsh
Looking for original zsh config...
Found ~/.zshrc.pre-oh-my-zsh -- Restoring to ~/.zshrc
Found ~/.zshrc -- Renaming to ~/.zshrc.omz-uninstalled-20170820200007
Your original zsh config was restored. Please restart your session.
Thanks for trying out Oh My Zsh. It's been uninstalled.
```
## 可能遇到的问题

以下是可能遇到的小问题

### 与vim的提示相冲突

使用自动补全插件可能会与vim的提示功能相冲突，如会报以下错误：

```bash
$ vim t
_arguments:451: _vim_files: function definition file not found
```

Bash

解决方法：将`~/.zcompdump*`删除即可

```bash
$ rm -rf ~/.zcompdump*
$ exec zsh
```

Bash

### 卡顿

有时候执行 `cd` 会卡，这让人难以忍受。原因是 `oh-my-zsh` 在获取 git 信息，可以将 git 信息隐藏：

```bash
git config --global oh-my-zsh.hide-status 1
```

Bash

## 小结

关于 `oh-my-zsh` 的功能，本文所涉及的仅是冰山一角，这些已经足够我平时使用了，极大提高了效率，相信我，一旦用过这个尤物，你再也不能忍受单调枯燥的 `bash` 了
