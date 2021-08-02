---
title: Linux 配置 zsh 并使用 oh-my-zsh
date: 2021-07-03 21:21
updated: # 更新日期
tags:
    - Ubuntu
    - zsh
categories: Linux
keywords:      # [可选] 文章关键字
description:   #  [可选] 描述 
top_img:       # 【可選】文章頂部圖片
comments:      # 【可選】顯示文章評論模塊(默認 true)
cover: https://z3.ax1x.com/2021/07/04/RWjpUP.png       # 【可選】文章縮略圖(如果沒有設置top_img,文章頁頂部將顯示縮略圖，可設為false/圖片地址/留空)
toc:           # 【可選】顯示文章TOC(默認為設置中toc的enable配置)
toc_number:    # 【可選】顯示toc_number(默認為設置中toc的number配置)
copyright: false   # 【可選】顯示文章版權模塊(默認為設置中post_copyright的enable配置)
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:      # 【可選】顯示mathjax(當設置mathjax的per_page: false時，才需要配置，默認 false)
katex:        # 【可選】顯示katex(當設置katex的per_page: false時，才需要配置，默認 false)
aplayer:      # 【可選】在需要的頁面加載aplayer的js和css,請參考文章下面的音樂 配置
highlight_shrink:   # 【可選】配置代碼框是否展開(true/false)(默認為設置中highlight_shrink的配置)
aside:        # 【可選】顯示側邊欄 (默認 true)
---

## 前言

Zsh 全称 Z shell 是一款可用作交互式登录的 shell 及脚本变现的命令解释器。Zsh 对 Bourne shell 做出了大量改进，同时加入了 Bash、ksh 及 tcsh 的某些功能。

自 2019 年起，macOS 的默认 Shell 已经从 Bash 改为 Zsh。

Zsh 的优势：

* 更丰富的命令提示
* 更鲜明的演示标记
* 更强大的插件支持

## 安装 zsh

查看系统内的所有 shell：

```shell
cat /etc/shells
```

若未安装，正常显示一般为：

```shell
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
```

如果你的 **Linux** 是**Ubuntu/Debian**，使用**apt-get**作为Linux软件包管理器，终端输入:

```shell
apt-get install zsh
```

**Centos**:

```shell
yum install zsh
```

安装后，重新查看 shell ：`cat /etc/shells` 

```shell
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/bin/zsh
/usr/bin/zsh
```

安装成功，设置默认并重启：

```shell
chsh -s /bin/zsh
```

## 配置Oh-my-zsh

zsh的功能极其强大，只是配置过于复杂，起初只有极客才在用。后来，有个穷极无聊的程序员可能是实在看不下去广大猿友一直只能使用单调的bash, 于是他创建了一个名为`oh-my-zsh`的[开源项目](https://github.com/ohmyzsh/ohmyzsh?source=c)

### 安装 Oh-my-zsh

为了保证脚本顺利运行，需要保证：

* 提前安装 `git`、`curl`
* 可以成功连接 Github
* 如果有 `~/.zshrc`文件，最好提前备份

| Method | conmmand                                                     |
| ------ | ------------------------------------------------------------ |
| curl   | sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" |
| wget   | sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" |
| fetch  | sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" |

选择上方任意一种方式安装即可

![官方方法安装](https://gitee.com/nanjingblue/typora-table/raw/master/installOhMyZshOfficial.png)

### Oh-my-zsh 换皮肤

在 `~/.zshrc`文件类修改主题，项目提供了大量主题供应，选择一款记住名字：[主题](https://github.com/ohmyzsh/ohmyzsh?source=c)

```shell
sudo vim ~/.zshrc
```

我这里使用的是 `agnoster` 主题，修改 `ZSH_THEME`

![image-20210703225255664](https://gitee.com/nanjingblue/typora-table/raw/master/image-20210703225255664.png)

修改主题后效果：

![image-20210703225447285](https://gitee.com/nanjingblue/typora-table/raw/master/image-20210703225447285.png)

### 多用户使用同一 zsh 配置

参考：https://askubuntu.com/questions/521469/oh-my-zsh-for-the-root-and-for-all-user

https://stackoverflow.com/questions/31624649/how-can-i-get-a-secure-system-wide-oh-my-zsh-configuration/42193058#42193058

`bash`不同用户都是读取`/etc/bash`文件作为环境配置文件，而`zsh`，默认是使用`$HOME`下的配置文件。那么，如果Linux上有多个用户怎么办？每次都要配置一遍？
原则上要，但是我们可以使用软连接，镜像某个用户的`zsh`配置到其他用户配置，这边演示**镜像pi用户的zsh配置到root用户**。
首先，创建软连接：

```
# 当前为pi用户
sudo ln -s $HOME/.oh-my-zsh /root/.oh-my-zsh
sudo ln -s $HOME/.zshrc /root/.zshrc

之后，更改pi用户就会自动映射给root用户，使两边zsh的配置一样。而所限于oh-my-zsh的配置，你需要开启`ZSH_DISABLE_COMPFIX`。

```
# 当前为pi用户
```shell
vim ~/.zshrc
```

加上`ZSH_DISABLE_COMPFIX=true`这条数据