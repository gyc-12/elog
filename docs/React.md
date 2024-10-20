---
title: React
urlname: tp4wl7vyhn3147p7
date: 2024-09-13T19:07:05.000Z
updated: '2024-10-17 11:24:02'
author: gaoyanchen
description: '---title: Reactdate: 2024-09-13 19:07:05tags: "笔记"thumbnail: "https://www.runoob.com/wp-content/uploads/2016/02/react.png"---colocation自动嵌套自动嵌套: 当你...'
tags: 笔记
thumbnail: 'https://www.runoob.com/wp-content/uploads/2016/02/react.png'
---
## colocation
![](https://raw.githubusercontent.com/gyc-12/images/master/41a5246605778574224978e0e8b6f6e2.png)

## 自动嵌套
+ **自动嵌套**: 当你在`**/dashboard**`目录下创建页面时，这些页面会自动作为`**children**`传递给`**<Layout />**`组件。这意味着这些页面会被渲染在布局组件定义的结构中。
+ ![](https://raw.githubusercontent.com/gyc-12/images/master/7f983d1bcab00c0c6c648bdf4c14f4c8.png)
+ ![](https://raw.githubusercontent.com/gyc-12/images/master/fb6c6cc9a9ae88ddefd2b3d65188898f.png)
+ ![](https://raw.githubusercontent.com/gyc-12/images/master/34df14fbd008ea1021367ef45c0ffefa.png)

## route groups
![](https://raw.githubusercontent.com/gyc-12/images/master/239ac73abdc5a988b7244d5d3c449cc4.png)

## state如同一张快照
**<font style="color:rgb(35, 39, 47);">设置 state 只会为下一次渲染变更 state 的值</font>**<font style="color:rgb(35, 39, 47);">。在第一次渲染期间，</font>`number`<font style="color:rgb(35, 39, 47);"> 为 </font>`0`<font style="color:rgb(35, 39, 47);">。这也就解释了为什么在 </font>**<font style="color:rgb(35, 39, 47);">那次渲染中的</font>**`onClick`<font style="color:rgb(35, 39, 47);"> 处理函数中，即便在调用了 </font>`setNumber(number + 1)`<font style="color:rgb(35, 39, 47);"> 之后，</font>`number`<font style="color:rgb(35, 39, 47);"> 的值也仍然是 </font>`0`<font style="color:rgb(35, 39, 47);">：</font>

```plain
<button onClick={() => {

  setNumber(number + 1);

  setNumber(number + 1);

  setNumber(number + 1);

}}>+3</button>
```

<font style="color:rgb(35, 39, 47);">以下是这个按钮的点击事件处理函数通知 React 要做的事情：</font>

1. `setNumber(number + 1)`<font style="color:rgb(35, 39, 47);">：</font>`number`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">是</font><font style="color:rgb(35, 39, 47);"> </font>`0`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">所以</font><font style="color:rgb(35, 39, 47);"> </font>`setNumber(0 + 1)`<font style="color:rgb(35, 39, 47);">。</font>
    - <font style="color:rgb(35, 39, 47);">React 准备在下一次渲染时将</font><font style="color:rgb(35, 39, 47);"> </font>`number`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">更改为</font><font style="color:rgb(35, 39, 47);"> </font>`1`<font style="color:rgb(35, 39, 47);">。</font>
2. `setNumber(number + 1)`<font style="color:rgb(35, 39, 47);">：</font>`number`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">是</font>`0`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">所以</font><font style="color:rgb(35, 39, 47);"> </font>`setNumber(0 + 1)`<font style="color:rgb(35, 39, 47);">。</font>
    - <font style="color:rgb(35, 39, 47);">React 准备在下一次渲染时将</font><font style="color:rgb(35, 39, 47);"> </font>`number`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">更改为</font><font style="color:rgb(35, 39, 47);"> </font>`1`<font style="color:rgb(35, 39, 47);">。</font>
3. `setNumber(number + 1)`<font style="color:rgb(35, 39, 47);">：</font>`number`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">是</font>`0`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">所以</font><font style="color:rgb(35, 39, 47);"> </font>`setNumber(0 + 1)`<font style="color:rgb(35, 39, 47);">。</font>
    - <font style="color:rgb(35, 39, 47);">React 准备在下一次渲染时将</font><font style="color:rgb(35, 39, 47);"> </font>`number`<font style="color:rgb(35, 39, 47);"> </font><font style="color:rgb(35, 39, 47);">更改为</font><font style="color:rgb(35, 39, 47);"> </font>`1`<font style="color:rgb(35, 39, 47);">。</font>

<font style="color:rgb(35, 39, 47);">尽管你调用了三次 </font>`setNumber(number + 1)`<font style="color:rgb(35, 39, 47);">，但在 </font>**<font style="color:rgb(35, 39, 47);">这次渲染的</font>**<font style="color:rgb(35, 39, 47);"> 事件处理函数中 </font>`number`<font style="color:rgb(35, 39, 47);"> 会一直是 </font>`0`<font style="color:rgb(35, 39, 47);">，所以你会三次将 state 设置成 </font>`1`<font style="color:rgb(35, 39, 47);">。这就是为什么在你的事件处理函数执行完以后，React 重新渲染的组件中的 </font>`number`<font style="color:rgb(35, 39, 47);"> 等于 </font>`1`<font style="color:rgb(35, 39, 47);"> 而不是 </font>`3`<font style="color:rgb(35, 39, 47);">。</font>

## <font style="color:rgb(35, 39, 47);">随时间变化的 state</font>
![](https://raw.githubusercontent.com/gyc-12/images/master/1bd9c92d14e531373282343ba8fb0a6d.png)

**<font style="color:#DF2A3F;">一个 state 变量的值永远不会在一次渲染的内部发生变化，</font>**<font style="color:#DF2A3F;"> 即使其事件处理函数的代码是异步的。在 </font>**<font style="color:#DF2A3F;">那次渲染的</font>**`<font style="color:#DF2A3F;">onClick</font>`<font style="color:#DF2A3F;"> 内部，</font>`<font style="color:#DF2A3F;">number</font>`<font style="color:#DF2A3F;"> 的值即使在调用 </font>`<font style="color:#DF2A3F;">setNumber(number + 5)</font>`<font style="color:#DF2A3F;"> 之后也还是 </font>`<font style="color:#DF2A3F;">0</font>`<font style="color:#DF2A3F;">。它的值在 React 通过调用你的组件“获取 UI 的快照”时就被“固定”了。</font>

<font style="color:#DF2A3F;"></font>

## <font style="color:#DF2A3F;">state</font>
<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">像这样的代码是有问题的，因为它改变了 state 中现有的对象：</font>

```plain
position.x = e.clientX;

position.y = e.clientY;
```

<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">但是像这样的代码就 </font>**<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">没有任何问题</font>**<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">，因为你改变的是你刚刚创建的一个新的对象：</font>

```plain
const nextPosition = {};

nextPosition.x = e.clientX;

nextPosition.y = e.clientY;

setPosition(nextPosition);
```

<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">事实上，它完全等同于下面这种写法：</font>

```plain
setPosition({

  x: e.clientX,

  y: e.clientY

});
```

<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">只有当你改变已经处于 state 中的 </font>**<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">现有</font>**<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);"> 对象时，mutation 才会成为问题。而修改一个你刚刚创建的对象就不会出现任何问题，因为 </font>**<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">还没有其他的代码引用它</font>**<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">。改变它并不会意外地影响到依赖它的东西。这叫做“局部 mutation”。你甚至可以 </font>[<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">在渲染的过程中</font>](https://zh-hans.react.dev/learn/keeping-components-pure#local-mutation-your-components-little-secret)<font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);"> 进行“局部 mutation”的操作。这种操作既便捷又没有任何问题！</font>

## <font style="color:rgb(35, 39, 47);background-color:rgb(243, 244, 253);">...</font>
    setPerson({

      ...person,

      email: e.target.value

    });

<font style="color:rgb(35, 39, 47);">请注意 </font>`...`<font style="color:rgb(35, 39, 47);"> 展开语法本质是是“浅拷贝”——它只会复制一层。这使得它的执行速度很快，但是也意味着当你想要更新一个嵌套属性时，你必须得多次使用展开语法。</font>

<font style="color:rgb(35, 39, 47);"></font>

## <font style="color:rgb(35, 39, 47);">immer</font>
<font style="color:rgb(35, 39, 47);">通过使用 Immer，你写出的代码看起来就像是你“打破了规则”而直接修改了对象：</font>

```plain
  const [person, updatePerson] = useImmer(.....)

updatePerson(draft => {

  draft.artwork.city = 'Lagos';

});
```

<font style="color:rgb(35, 39, 47);">但是不同于一般的 mutation，它并不会覆盖之前的 state！</font>

<font style="color:rgb(35, 39, 47);">请注意当使用 Immer 时，</font>**<font style="color:rgb(35, 39, 47);">类似 </font>**`**artwork.seen = nextSeen**`**<font style="color:rgb(35, 39, 47);"> 这种会产生 mutation 的语法不会再有任何问题了：</font>**

```plain
updateMyTodos(draft => {

  const artwork = draft.find(a => a.id === artworkId);

  artwork.seen = nextSeen;

});
```

<font style="color:rgb(35, 39, 47);">这是因为你并不是在直接修改原始的 state，而是在修改 Immer 提供的一个特殊的 </font>`draft`<font style="color:rgb(35, 39, 47);"> 对象。同理，你也可以为 </font>`draft`<font style="color:rgb(35, 39, 47);"> 的内容使用 </font>`push()`<font style="color:rgb(35, 39, 47);"> 和 </font>`pop()`<font style="color:rgb(35, 39, 47);"> 这些会直接修改原值的方法。</font>

<font style="color:rgb(35, 39, 47);">在幕后，Immer 总是会根据你对 </font>`draft`<font style="color:rgb(35, 39, 47);"> 的修改来从头开始构建下一个 state。这使得你的事件处理程序非常的简洁，同时也不会直接修改 state。</font>

<font style="color:rgb(35, 39, 47);"></font>

## <font style="color:rgb(35, 39, 47);">==和===</font>
![](https://raw.githubusercontent.com/gyc-12/images/master/e4701dcad2b72bc58e9ab9f2344f509b.png)

## `<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">和</font><font style="color:rgb(25, 27, 31);"> </font>`<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMomeo</font>`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">的区别</font>
`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">和</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">都是用于性能优化的 React 钩子函数，它们都可以避免不必要的重新计算或重新渲染。虽然它们看起来很相似，但它们有几个重要的区别。</font>

<font style="color:rgb(25, 27, 31);">首先，</font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">返回一个缓存的回调函数，而</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">返回一个缓存的值。这意味着</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">的主要作用是为一个函数创建缓存，而</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">的主要作用是缓存一个值。</font>

<font style="color:rgb(25, 27, 31);">其次，它们接受的参数不同。</font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">接受一个回调函数和一个依赖项数组，而</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">接受一个函数和一个依赖项数组。在</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">中，只有依赖项数组中的值发生变化时，才会重新创建回调函数。而在</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">中，只有依赖项数组中的值发生变化时，才会重新计算值。</font>

<font style="color:rgb(25, 27, 31);">最后，它们的使用场景也不同。</font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">适用于优化回调函数，避免不必要的重新渲染，并传递给子组件。而</font><font style="color:rgb(25, 27, 31);"> </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> </font><font style="color:rgb(25, 27, 31);">适用于优化计算开销较大的值，如大型数组或对象的计算。</font>

<font style="color:rgb(25, 27, 31);">综上所述，</font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useCallback</font>**`<font style="color:rgb(25, 27, 31);"> 和 </font>`**<font style="color:rgb(25, 27, 31);background-color:rgb(248, 248, 250);">useMemo</font>**`<font style="color:rgb(25, 27, 31);"> 的主要区别在于它们的返回值类型和使用场景。需要根据具体的情况选择使用哪个钩子函数。</font>

