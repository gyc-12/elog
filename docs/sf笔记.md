---
title: sf笔记
urlname: zbwytv6yvg4vym0w
date: 2024-09-13T19:07:05.000Z
updated: '2024-09-13 21:09:12'
author: gaoyanchen
description: '---title: sf笔记date: 2024-09-13 19:07:05tags: "笔记"thumbnail: "https://www.apple.com.cn/newsroom/images/logos/quick-reads-logos/Apple-logo.jpg.square...'
tags: 笔记
thumbnail: 'https://www.apple.com.cn/newsroom/images/logos/quick-reads-logos/Apple-logo.jpg.square_social.jpg'
---
+ 基本原理: D*算法的核心思想是在机器人移动过程中,根据新发现的环境信息动态更新路径。它维护一个从目标到起点的最优路径,并在发现新障碍物时进行局部修改。
+ 关键概念:
+  a) 状态: 表示环境中的位置。
+  b) 代价: 从一个状态到另一个状态的移动代价。
+  c) 标签: 每个状态可能被标记为NEW(新建)、OPEN(开放)或CLOSED(关闭)。 
+ d) 键值(key): 用于确定状态处理顺序的优先级值。

算法步骤: 

a) 初始化: 

+ 将目标状态插入OPEN列表。
+ 设置目标状态的代价为0。
+ 所有其他状态的代价初始化为无穷大。

 b) 主循环: 

+ 从OPEN列表中选择键值最小的状态X。
+ 如果X是起始状态且其代价小于无穷大,则找到路径。
+ 否则,展开状态X: 
    - 对X的每个相邻状态Y: 
        * 计算从X到Y的新代价。
        * 如果新代价小于Y的当前代价,更新Y的代价和后继状态。
        * 将Y插入或移动到OPEN列表。

 c) 路径跟踪: 

+ 从起始状态开始,每次选择代价最小的相邻状态,直到到达目标。

 d) 动态更新: 

+ 当发现新障碍物时: 
    - 更新受影响状态的代价。
    - 将这些状态重新插入OPEN列表。
+ 重新执行主循环,更新路径。



![](https://raw.githubusercontent.com/gyc-12/images/master/a222bf10d10b7af0dcfae75f8680f9a7.svg)



