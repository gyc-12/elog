---
title: lightroom分析学习
date: 2024-8-24 19:07:05
tags: "逆向"
thumbnail: "https://helpx.adobe.com/content/dam/help/mnemonics/Lr_cc_appicon_noshadow_2017.svg"
---
起因
本来用qiuchenly大佬的注入库，注入后应用程序崩溃，应该是代码没有适配最新的lightroom 7.2，又不想降级应用，所以就自己研究看看怎么实现的。

## 准备工作
vscode
Hopper Disassemble
ida free 8.4
![](https://s2.loli.net/2024/04/10/PhBis1nL7I6YdTr.png#id=TihK1&originHeight=1976&originWidth=3384&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

## 开始分析
先在vscode搜索关键字 ：您的试用期限已结束，编辑功能已禁用 现在购买
![](https://s2.loli.net/2024/04/10/qEXfJIUV6pkWLdS.png#id=Nd5o3&originHeight=2072&originWidth=3912&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
发现没有什么实际有用的String
尝试在hopper中直接搜索中文也无果
![](https://s2.loli.net/2024/04/10/K3u7LGrDzJ29BAw.png#id=uLccB&originHeight=2070&originWidth=3912&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
尝试搜索试用的英文：Trial
![](https://s2.loli.net/2024/04/10/9gUPWKYNFHXtxTG.png#id=GJSSn&originHeight=2070&originWidth=3912&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
发现有用的字符串，按x查看引用，找到关键函数：get_licenseType
![](https://s2.loli.net/2024/04/10/duOn7U8Ee6oMjwQ.png#id=ID1DV&originHeight=1586&originWidth=3000&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
可以看到如果走到 loc_1001197c6 是trial_expired，如果走到loc_10011972c,就是trial模式，所以思路就是让程序默认走到trial的函数。
![](https://s2.loli.net/2024/04/10/e8Dl2KbMRVYhZwO.png#id=bJLmL&originHeight=2068&originWidth=3910&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
cfg模式可以看到 ，trial函数有两条路可以跳转，当然优先在上一级就直接跳转过来，我们直接修改000000010011970e        **je**        loc_10011972c  ----->>        **jne**        loc_10011972c
 ![](https://s2.loli.net/2024/04/10/NFpY6o9MTZlxmtz.png#id=e2euQ&originHeight=2072&originWidth=3912&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
然后直接 保存打包替换启动！编辑功能已经可用。
![](https://s2.loli.net/2024/04/10/TBsx4Rbfjn67cMJ.png#id=xjtt3&originHeight=2060&originWidth=3904&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
破解流程比较简单，可以使用qiuchenly大佬的注入库，搞成优雅的注入破解。
