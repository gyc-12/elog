---
title: macos insert_lib命令
urlname: bch4f1d7a20lccoz
date: 2024-09-03T19:07:05.000Z
updated: '2024-09-06 14:55:56'
author: gaoyanchen
description: '---title: macos insert_lib命令date: 2024-9-03 19:07:05tags: "笔记"thumbnail: "https://avatars.githubusercontent.com/u/74996459?v=4"---macos insert_lib命...'
tags: 笔记
thumbnail: 'https://avatars.githubusercontent.com/u/74996459?v=4'
---
macos insert_lib命令

<font style="color:rgb(34, 34, 34);"></font>

<font style="color:rgb(34, 34, 34);"></font>

```shell
#首先查看原本的注入方式

otool  -L /executable/....

# 注入新的

sudo  /Users/name/PycharmProjects/InjectLib/tool/insert_dylib  @rpath/macked.app.dylib   /Applications/MediaMate.app/Contents/Frameworks/SwiftUIHidden.framework/Versions/A/SwiftUIHidden
```





