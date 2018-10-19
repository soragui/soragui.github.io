---
layout: post
image: flutter.png
---
> 对Flutter跨平台的信息是越来越大了，进过了这么长时间的考察，决定以后移动端的开发就用它了(其实这段时间一直没怎么看)。首先就从最简单的 Row 和 Cloumn 开始吧。

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
[<img src="assets/images/rowcolumn/basic.png" align="center" width="850">]()