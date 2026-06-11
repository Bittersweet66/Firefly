---
title: 基于AstrBot和Napcat的QQBot开发
published: 2026-06-11
description: 这是关于用astrbot开发一个属于你的qqbot的一篇介绍。
image: /assets/images/cover.avif
tags: [Napcat, AstrBot，开发，Agent]
category: bot开发
author: Nomain
draft: false
slug: how-to-deploy-QQBot
---
**引言**：最近开发了一个QQbot，可以自动回复聊天，还可以查询视频等。本文来分享我的开发历程，希望对你有所帮助。

## 1.准备工作
### 1.1 下载Astrbot
打开 <https://docs.astrbot.app>，点击左侧的“启动器一键部署”，下翻找到github仓库，进入。
![1](/assets/images/md1_1.png)

下载最新版的AstrBotLauncher.zip文件，解压放到自己喜欢的地方。


### 1.2 下载Napcat
打开 <https://napneko.github.io/guide/start-install>,在左侧安装方式找到**Shell**，点击。

进入后，下翻找到**NapCat.Win.一键版本**里的**NapCatQQ 的 Releases 页面**，进入github页面获取。

下载Windows OneKey版本的.zip文件，和astrBot放到同个文件夹（尽量），同样解压。


### 1.3 部署本地Napcat和AstrBot
点开napcat的文件夹，打开**NapCatInstaller.exe**（注意先解除锁定），放在一边。

再打开AstrBot文件夹里的**launcher_astrbot_en.bat**，然后等待这两个安装完成。

安装完成之后，关闭Napcat的程序，再在NapCat文件夹里找到**NapCat.44498.Shell**这个文件夹，打开，运行**napcat.bat**脚本。
![2](/assets/images/md1_2.png)

Ctrl点击图中**127**开头的网址，进入登陆界面，第一次登陆账号密码在日志里，进去后会让你更改。

再Ctrl点击Napcat脚本里的**127**开头的网址，进入NapCat网站，登录你想要变成bot的qq。
![3](/assets/images/md1_3.png)

点击左侧的网络配置，左上角**新建**，点击**WebSocket服务器**，把所有按钮都打开，将**Host**改为0.0.0.0（局域网访问），随便取个名字，删除token（这样方便点）。

再新建一个，点击**WebSocket客户端**，打开所有按钮，将URL改为**ws://127.0.0.1:6199/ws**，，同样删除token。

去**实施调试**里启用**链接**，这样就好了。


## 2.创立Bot
### 2.1 在Astrbot里建立bot
来到AstrBot仪表盘，点击左边栏**机器人**，点击**创建机器人**，在机器人类型里选择**OneBot**，启用一下，保存，等待适配器链接即可。


### 2.2 设置Bot模型提供商
点击左边栏**模型提供商**，新建一个，选择你想要配置的模型（推荐deepseek-chat），输入你的模型APIKey，保存配置。

点击下方**获取模型列表**，按自己需求添加列表里模型，并启用。


### 2.3 配置Bot基本属性
点击左边栏**配置**，在**默认对话模型**里选择你要的模型，其他模型按需选择。

往下翻找到**人格**，这里可以设置模型的人格，拟写一些提示词，改变Bot说话习惯。

都配置好后，记得点击右下角保存配置。此时就可以用了，可以打开qq和她对话试试。

注意到左边栏的**插件市场**，在里面可以挑选喜欢的插件来增强Bot的功能，这个按需选择。

此时一个比较精美的Bot就创建好了，可以和她聊天，也可以拉进群聊里和群成员互动。
每次想要启动bot，启动AstrBot_Launcher和napcat.bat就可以了。


## Tips ！！
如果想让bot有视觉效果，例如读取视频、图片，就要在配置里选择带有视觉解析的模型（例如千问等），然后去插件市场寻找相关插件配置。

## 附录（视频操作）
<iframe width="100%" height="468"
  src="https://www.bilibili.com/video/BV1zRfEBeEpj/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=39d291e465fcf57d072c502787a2043b"
  scrolling="no" border="0" frameborder="no"
  framespacing="0" allowfullscreen="true">
</iframe>