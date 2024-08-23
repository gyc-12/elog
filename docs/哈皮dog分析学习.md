
# 哈皮dog分析学习
新人学习报告，恳请各位大佬指正。本文仅用于学习逆向知识，不可违法利用。他人以此牟利与本人无关。如有侵权，请联系立即删除。

## 使用工具

- VSCode
- Hopper Disassembler

## 分析过程

### 1. 初步搜索
打开应用目录搜索"专业版"，发现多个相关字符串。
![](https://raw.githubusercontent.com/gyc-12/images/master/a8f5b5331606f061c7fe2fca838bb802.png)

### 2. Hopper分析
在Hopper中搜索相关引用。
![](https://raw.githubusercontent.com/gyc-12/images/master/2c68e7b5e7e91c2ea0c7e7f588516297.png)
只发现一些视图操作，未见明显业务逻辑判断。
![](https://raw.githubusercontent.com/gyc-12/images/master/f5f6158c0a98daec7235ae7bcad85acc.png)

### 3. 断点调试
尝试设置断点查看堆栈，但效果不佳。
![](https://raw.githubusercontent.com/gyc-12/images/master/047d6562d8a2b64b4996e3d3cf8738f4.png)

### 4. 更换搜索目标
搜索"_SUBSCRIBE"，发现激活pro后才显示的内容。
![](https://raw.githubusercontent.com/gyc-12/images/master/7d4c6c5a77cd960606d6d6ecf0138171.png)

### 5. 关键判断逻辑
发现明显的判断逻辑。
![](https://raw.githubusercontent.com/gyc-12/images/master/e2d21f33b234f1b5d1bca2fa70337a9e.png)

### 6. 修改寄存器值
在关键位置设置断点，修改寄存器值：`register write al 0x1`
![](https://raw.githubusercontent.com/gyc-12/images/master/f8fa1fa6403fe64c1637604e609ec6ca.png)

### 7. 分析关键函数
分析`sub_100879740()`函数，虽然逻辑复杂，但很可能是授权相关函数。
![](https://attach.52pojie.cn/forum/202406/19/193444r03c5svslcoev01n.png#id=jhMft&originHeight=490&originWidth=1464&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

### 8. 最终修改
尝试让`sub_100879740()`直接返回1。
![](https://raw.githubusercontent.com/gyc-12/images/master/8a15b02b80792306cdba6605f3cd1cd4.png)

## 结果
打包重启后，本地功能全部可用。
![](https://attach.52pojie.cn/forum/202406/19/183109w0g11ace1weauaaa.png#id=ZfRel&originHeight=1132&originWidth=1520&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)

## 总结
本次分析过程中运气成分较大。感谢秋佬、v佬的帖子，受益良多。
![](https://raw.githubusercontent.com/gyc-12/images/master/3be6a56aeffe8a69d22db079c3924f0f.png)
