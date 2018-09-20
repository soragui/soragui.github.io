---
layout: post
image: flutter.png
---
>把自己的所思所想，所做所为写成博客的形式确实有不少的好处。其中最明显的就是可以整理自己的学习思路，检测自己对知识技术的理解和掌握程度，最重要的是还有一种成就感。不过最重要的就是要坚持写...这一点我很是欠缺...😕

### 序

不知不觉我已经接触Flutter两个多月了，说实话总体来说还是比较满意的。所以要花一点时间整理一下这两个月来所学习到的东西。

两个月的时间应该写了近一万行的代码，接触最多的就是Flutter 中的 Widget，几乎所有的事情都是在这里完成的，在界面设计上有些许的收获，然而在代码逻辑、软件设计上并没有多大的提成。

今天这里就首先来回顾一下Flutter中基本Widget的使用。不过在开始之前，还是要复习一下Flutter的整体结构，我喜欢先把握全局，然后再各个击破，我要弄清楚我写的代码到底干了什么....这样才不会迷茫...

![Flutter Architecture](/assets/images/basicwidget/flutter_architecture.png)

上图就是Flutter的架构了，以后还会经常复习，Widgets 属于第二层，在我眼里它就是负责UI的...当然实际上不止这些。

学习和复习都是参考 [Flutter Widget Catalog Basics](https://flutter.io/widgets/basics/),只是关注的点不一样而已，传说中的温故而知新 😛 .

Flutter 共有11个基础Widget，官方说这是你开始上手Flutter APP所必须了解的Widget。经过这两个月来的实践，对这些Widget总算有一些了解，不过还有很多不熟悉的地方，下面就来简单说一些使用体会。

### Scafflod
Scaffold 是最基础的的UI组件，仍然在上次的HW的基础上学习，其代码调用流程如下图所示：

```dart
    main() => runApp( MyApp() )
                        ||
                        \/
                return MaterialApp(
                    title: ...
                    theme: ...
                    home: MyHomePage()
                )           ||
                            \/
                 return _MyHomePageState()  
                                ||
                                \/
                       return Scaffold(
                           appBar: ...
                           body: ...
                           floatingActionButton: ...
                       )                  
```                  

此 Scaffold 中传入了三个参数分别是 appBar、body和floatingActionButton。还有 drawer、bottomAppBar 和 bottomNavigationBar会经常用到。

首先为了了解这些参数的作用和位置，我们在MaterialApp的home参数传入一个空的 Scaffold() 看看会发生什么：
```dart
home: Scaffold()
```
#### 添加 AppBar

不出所料，是一个白板，好的下面就来添加一个AppBar，如下所示：

```dart
Scaffold(
    appBar: AppBar()
)
```

其中AppBar的背景颜色可以在 MaterialApp() 的 theme 参数中由 primarySwatch 参数指定，现在你可以看到一个AppBar了，如下图所示：

![addAppBar](/assets/images/basicwidget/addappbar.png)

现在可以向AppBar中添加一些 title 什么的，当然 还有很多要说的具体下面单独看看。

```dart
appBar: AppBar(
    title: Text('Nothing At ALl')
)
```

#### 添加 body
body 是一个Widget，也就是说你可以传入任何一个你想要的Widget。例如添加一个Text:

```dart
body: Text('BODY'),
```

![text](/assets/images/basicwidget/text.png)

不过这个BOdy好丑，我想把它显示在中间，在Flutter中有很多方式可以实现，不过最简单的就是用Center()包住了:

```dart
body: Center(
    child: Text('BODY'),
)
```

![text](/assets/images/basicwidget/centertext.png)

#### 添加 bottomNavigationBar
bottomNavigationBar 就是底部导航栏，不过添加这个navigationBar并没有想象中的那么简单直接。那我们还是从头开始吧！

首先 bottomNavigationBar 也是一个 Widget，只不过它默认的位置是底部，那么我们也添加一个 Text 看看是什么结果：

```dart
bottomNavigationBar: Text('bottomNavigationBar'),
```

![text](/assets/images/basicwidget/bottomtext.png)
好吧，这个虽然可以，但好像并不是很美观，我们还是添加一个 NavigationBar 吧。首先Flutter 自带了一个 BottomNavigationBar 这个 Widget，那么我们就用这个看看到底是怎么回事：

```dart
bottomNavigationBar: BottomNavigationBar(

    items: => <BottomNavigationBarItem>[]  // BNBI 数组
    currentIndex:  // 指定当前显示的 items 下标 
    onTap: (value) {}  // 监听函数 

)
```

可以看出，bNB 总共有三个重要的参数，一个 items index 显示 一个 监听函数，如果需要和相应的页面对应相应的 item 点击事件，那么就需要自己控制整个 index ，根据 index 的改变来显示相应的页面...



逻辑不是很清晰，就先到这里了 .... 😂
