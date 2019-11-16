---
layout: post
image: assets/images/flutter.png
comments: true
---
> 对Flutter跨平台的信息是越来越大了，进过了这么长时间的考察，决定以后移动端的开发就用它了(其实这段时间一直没怎么看)。首先就从最简单的 Row 和 Column 开始吧。

Row 和 Column 是 Flutter 种最常用的布局Widget，其中 Row 是按水平方向显示子Widget，而Column则是按垂直方向显示。

下面我们就来一步一步了解其中有哪些可用的属性值得我们关注。为了简单，我这里只是写了Row相关的例子，Column 和 Row 的一些属性具有对称性。

最简单的 Row 如下所示，传入了一个 *chaildren : <Widget>[]* 参数，子Widget数组，其成员可以是任意的Widget。

```dart
child: Row(

          children: <Widget>[
            
            Container(
              width: 30.0,
              height: 30.0,
              color: Colors.black,
            ),
            Container(
              width: 30.0,
              height: 10.0,
              color: Colors.red,
            ),
            Container(
              width: 30.0,
              height: 20.0,
              color: Colors.green,
            ),

          ],

        ),
```

这个Row总共有三个Container子Widget，一个高度为30背景色为黑，一个高度为10背景色为红，一个高度为20背景色为绿，之所以用不同的高度，这样就可以看出Widget之间的对其方式，效果图如下所示：
![](/assets/images/rowcolumn/basic.png)

Row 的构造函数如下所示：

```dart
Row({
    Key key,
    MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
    MainAxisSize mainAxisSize = MainAxisSize.max,
    CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
    TextDirection textDirection,
    VerticalDirection verticalDirection = VerticalDirection.down,
    TextBaseline textBaseline,
    List<Widget> children = const <Widget>[],
  })
```

其中 key 相当于一个指针对象，以后你可以用这个key来获取这个Row的一些属性。MainAxisAlignment 和 CrossAxisAlignment 分别表示主对角线对齐方式和副对角线对齐方式，在Row中就是 水平对齐方式和垂直对齐方式，那么在Column中就是垂直对齐方式和水平对齐方式。同样的MainAxisSize 在 Row 中表示水平方向上占用空间的大小，在 Column中表示在垂直方向上占用空间的大小。TextDirection 和 VerticalDirection 是选择对齐方向从哪里开始的(反正我是这么理解的)。好了下面就从具体的例子来看看这些参数是如何发挥作用的：

```dart
Row (

  mainAxisAlignment: MainAxisAlignment.center // 分别修改 mainAxisAlignment 为 center start end 

  children: <Widget>[
    ....
  ]

)
```
#### mainAxisAlignment.start
![start](/assets/images/rowcolumn/rmstart.png)
#### mainAxisAlignment.center
![center](/assets/images/rowcolumn/rmcenter.png)
#### mainAxisAlignment.end
![end](/assets/images/rowcolumn/rmend.png)

下面把 mainAxisAlignment 设置为 center 来看一下 crossAxisAlignment 的属性是如何工作的：

```dart
Row (

  mainAxisAlignment: MainAxisAlignment.center ,
  corssAxisAlignment: CrossAxisAlignment.start ,// 分别修改 crossAxisAlignment 为 center start end 

  children: <Widget>[
    ....
  ]

)
```
#### crossAxisAlignment.start
![start](/assets/images/rowcolumn/rcstart.png)
#### crossAxisAlignment.center
![center](/assets/images/rowcolumn/rmcenter.png)
#### crossAxisAlignment.end
![end](/assets/images/rowcolumn/rcend.png)

Column 和 Row 是对应的，他们之间互为对称关系。

还有 Expanded 和 Flex 没有想好怎么写，好乱 - 下次继续努力 🙂
