<font style="color:rgb(34, 34, 34);">macos insert_lib命令</font>

<font style="color:rgb(34, 34, 34);"></font>

<font style="color:rgb(34, 34, 34);"></font>

```shell
#首先查看原本的注入方式

otool  -L /executable/....

# 注入新的

sudo  /Users/name/PycharmProjects/InjectLib/tool/insert_dylib  @rpath/macked.app.dylib   /Applications/MediaMate.app/Contents/Frameworks/SwiftUIHidden.framework/Versions/A/SwiftUIHidden
```





