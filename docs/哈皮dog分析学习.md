---
title: 哈皮dog分析学习
date: 2024-8-24 19:07:05
tags: "逆向"
thumbnail: "https://hapigo.com/static/image/logo.svg"
---
新人学习报告，恳请各位大佬指正。本文仅用于学习逆向知识，不可违法利用。他人以此牟利与本人无关。如有侵权，请联系立即删除。

## 使用工具

- VSCode
- Hopper Disassembler

## 分析过程

### 1. 初步搜索
打开应用目录搜索"专业版"，发现多个相关字符串。
![](https://attach.52pojie.cn/forum/202406/19/183445v04uqkiu4xz40zrc.png#id=TwK54&originHeight=2072&originWidth=3912&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 2. Hopper分析
在Hopper中搜索相关引用。
![](https://attach.52pojie.cn/forum/202406/19/184351qt29bhns2uylqyys.png#id=NBApx&originHeight=2072&originWidth=3912&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
只发现一些视图操作，未见明显业务逻辑判断。
![](https://attach.52pojie.cn/forum/202406/19/184533ok3gzj42ltihz1zj.png#id=IRUgl&originHeight=1362&originWidth=3804&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 3. 断点调试
尝试设置断点查看堆栈，但效果不佳。
![](https://attach.52pojie.cn/forum/202406/19/185403of1zmihubz0zc7r2.png#id=bx2KI&originHeight=1468&originWidth=3646&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 4. 更换搜索目标
搜索"_SUBSCRIBE"，发现激活pro后才显示的内容。
![](https://attach.52pojie.cn/forum/202406/19/193153upirj7pwjouxzidz.png#id=nLaoM&originHeight=1548&originWidth=3528&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 5. 关键判断逻辑
发现明显的判断逻辑。
![](https://attach.52pojie.cn/forum/202406/19/185516bthr8reeieoe7hjh.png#id=Gmh0l&originHeight=1156&originWidth=3794&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 6. 修改寄存器值
在关键位置设置断点，修改寄存器值：`register write al 0x1`
![](https://attach.52pojie.cn/forum/202406/19/193257fa3kpmf3obmjaoko.png#id=XpfSO&originHeight=1508&originWidth=3324&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 7. 分析关键函数
分析`sub_100879740()`函数，虽然逻辑复杂，但很可能是授权相关函数。
![](https://attach.52pojie.cn/forum/202406/19/193444r03c5svslcoev01n.png#id=jhMft&originHeight=490&originWidth=1464&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 8. 最终修改
尝试让`sub_100879740()`直接返回1。
![](https://attach.52pojie.cn/forum/202406/19/193357gavgmmedq8mlitmq.png#id=gEMzj&originHeight=1148&originWidth=2526&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

## 结果
打包重启后，本地功能全部可用。
![](https://attach.52pojie.cn/forum/202406/19/183109w0g11ace1weauaaa.png#id=ZfRel&originHeight=1132&originWidth=1520&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

## 总结
本次分析过程中运气成分较大。感谢秋佬、v佬的帖子，受益良多。
![](https://attach.52pojie.cn/forum/202406/19/185805r6akzkvdnp96zv8o.png#id=SQN2L&originHeight=1164&originWidth=3724&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=iShot_2024-06-19_18.57.35.png)
