---
title: lightroom分析学习
date: 2024-8-24 19:07:05
tags: "逆向"
thumbnail: "[https://helpx.adobe.com/content/dam/help/mnemonics/Lr_cc_appicon_noshadow_2017.svg](https://helpx.adobe.com/content/dam/help/mnemonics/Lr_cc_appicon_noshadow_2017.svg)"
---
起因
本来用qiuchenly大佬的注入库，注入后应用程序崩溃，应该是代码没有适配最新的lightroom 7.2，又不想降级应用，所以就自己研究看看怎么实现的。

## 准备工作
vscode
Hopper Disassemble
ida free 8.4
![](https://raw.githubusercontent.com/gyc-12/images/master/78a9af1aa994e14c8de06a5e04e6b137.png)

## 开始分析
先在vscode搜索关键字 ：您的试用期限已结束，编辑功能已禁用 现在购买
![](https://raw.githubusercontent.com/gyc-12/images/master/af70a7db7fb1346af5599c91e79f0fbf.png)
发现没有什么实际有用的String
尝试在hopper中直接搜索中文也无果
![](https://raw.githubusercontent.com/gyc-12/images/master/fb4b062a36563e54ffecdf6bef802d10.png)
尝试搜索试用的英文：Trial
![](https://raw.githubusercontent.com/gyc-12/images/master/0050148df697cdbb73f7f3bf3b491e26.png)
发现有用的字符串，按x查看引用，找到关键函数：get_licenseType
![](https://raw.githubusercontent.com/gyc-12/images/master/808ba46c51f9c01496af8ba0e4141150.png)
可以看到如果走到 loc_1001197c6 是trial_expired，如果走到loc_10011972c,就是trial模式，所以思路就是让程序默认走到trial的函数。
![](https://raw.githubusercontent.com/gyc-12/images/master/9eca8ace2ba1a62c012f1f7a11f867c4.png)
cfg模式可以看到 ，trial函数有两条路可以跳转，当然优先在上一级就直接跳转过来，我们直接修改000000010011970e        **je**        loc_10011972c  ----->>        **jne**        loc_10011972c
 ![](https://raw.githubusercontent.com/gyc-12/images/master/c6c6570c4166d1c09884273e8b77e7e6.png)
然后直接 保存打包替换启动！编辑功能已经可用。
![](https://raw.githubusercontent.com/gyc-12/images/master/9a6c16de1579bc4896d5df3266bfdc73.png)
破解流程比较简单，可以使用qiuchenly大佬的注入库，搞成优雅的注入破解。
