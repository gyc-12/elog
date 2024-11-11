---

title: `ScrollView`和`FlatList`

date: 2024-11-11 19:07:05

tags: "笔记"

thumbnail: "https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/acom_social_icon_azure"

---

此外`FlatList`还可以方便地渲染行间分隔线，支持多列布局，无限滚动加载等等。

`ScrollView`和`FlatList`应该如何选择？ScrollView 会简单粗暴地把所有子元素一次性全部渲染出来。其原理浅显易懂，使用上自然也最简单。然而这样简单的渲染逻辑自然带来了性能上的不足。想象一下你有一个特别长的列表需要显示，可能有好几屏的高度。创建和渲染那些屏幕以外的 JS 组件和原生视图，显然对于渲染性能和内存占用都是一种极大的拖累和浪费。

这就是为什么我们还有专门的`FlatList`组件。`FlatList`会惰性渲染子元素，只在它们将要出现在屏幕中时开始渲染。这种惰性渲染逻辑要复杂很多，因而 API 在使用上也更为繁琐。除非你要渲染的数据特别少，否则你都应该尽量使用`FlatList`，哪怕它们用起来更麻烦。  


> 来自: [ScrollView · React Native 中文网](https://reactnative.cn/docs/scrollview)
>

