---  
title: macos insert_lib命令  
date: 2024-9-3 19:07:05  
tags: "笔记"  
thumbnail: "https://www.serv00.com/static/ct8/img/logo.jpg"  
---

<font style="color:rgb(34, 34, 34);">macos insert_lib命令</font>

<font style="color:rgb(34, 34, 34);"></font>

<font style="color:rgb(34, 34, 34);"></font>

```shell
#首先查看原本的注入方式

otool  -L /executable/....

# 注入新的

sudo  /Users/name/PycharmProjects/InjectLib/tool/insert_dylib  @rpath/macked.app.dylib   /Applications/MediaMate.app/Contents/Frameworks/SwiftUIHidden.framework/Versions/A/SwiftUIHidden
```





