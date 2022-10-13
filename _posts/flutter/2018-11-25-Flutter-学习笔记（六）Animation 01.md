---
layout: post
image: assets/images/flutter.png
comments: true
author: sal
categories: flutter
---
> 现在移动开发界面应该占了很大一部分，而且越简洁、越直接就越好，让人赏心悦目，而添加一些简单的动画在里面会提升与用户的交互感，下面就开始学习 Flutter 中的动画吧！

### 简介
刚接触 Flutter Animation 不久，说实话，还有好多东西没有学到。准确来说只是刚起步而已，其中的一些概念还不是很清楚，所以有的知识点可能写的模棱两可或者表述比较简单。在 Flutter 中，动画主要分为两种：Tween和Physics。第一种Tween是表示在两点之间发生的一些动画，这两点可以是时间或者状态，然后定义从第一点到第二点过度的动画以及如何过度、过度时长等。第二种就是基于现实物理规律的动画了，目前我只看了第一种Tween动画的一小部分，所以现在就简单说一下，顺便自己复习一下、整理一下思路。实际上这里只涉及到一些Flutter动画类API的使用，其原理我也没怎么弄明白，所以就不多说了。

### 基本概念
前面说过，动画有两种形式，这主要使用Tween。Tween<T> 也就是 in-between 的简写，Flutter 才刚起步，关于这方面的文档很少，所以只好看官方文档。然而，官方文档也是很抽象，因此就绕过把，只要知道这是构建动画的基础类。然后就是 Animation<T> 类，Tween<T> 类定义了两个 T 类型的状态，也就是 begin 和 end 两个状态，然后通过 Animation<T> 类来进行进一步的封装用来定义如果过度，也就是从 begin 到 end 的过度动画。最上层就是 AnimaitonController 了，这个就是定义动画持续的时间以及控制动画的开始和结束等状态、从 begin 到 end 或者反过来 从 end 到 begin。

在 Flutter 中自带的动画主要有如下四种表现形式：

 - Translation 可以理解为平行移动
 - Rotation 旋转运动
 - Scale  放大缩小
 - Opacity 更改透明度

你可以参考[官方文档](https://docs.flutter.io/flutter/animation/Curves-class.html)这个动画还是很清楚的。这里有一个 CurvedAnimation 类，它就是继承自 Animation<double> 类，也就是说它的 Tween 中要变化的类型就是一个 double 。默认是从 0.0 到 1.0。可以理解为在这之间发生的一些事情，即动画，也就是 Widget 的移动。

CurvedAnimation 参数如下：

```dart
CurvedAnimation({
    @required Animaiton<double> parent, 
    @required Curve curve,
    Curve reverseCurve,
})
```

这里的 parent 参数用于指定动画控制类，一般就是 AnimationController 类用来指定开始到结束的持续时间、一些监听函数以及控制动画的开始等，而 curve 和 reverseCurve 就是指定动画的形式，也就是上面官方给出的几种动画形式。

好了，现在来看一个简单的例子，前面说了 Translaiton 是四种动画表现形式中的一种，而 SizeTransition 就是其中的一个实现，它继承自 AnimatedWidget ，所以可以直接使用。如下所示，是它的参数：

```dart

SizeTransition({
    Key key,
    Axis axis: Axis.vertical,
    @required Animation<double> sizeFactor,
    double axisAlignment: 0.0,
    Widget child,
})

```

可以看出一个有一个  Animation<double> 参数指定动画形式等，而 Widget 可以是任何控件。下面来看看 AnimationController 是个什么东西：

```dart
AnimationController({
    @required TickerProvider vsync,
    Duration duration,
    ....
})
```

目前我们首先来看看这两个参数，一个是指定 Ticker，这个就是给一个定时器，需要 Widget 类继承一个 TickerProvider。而 duration 很好理解就是动画的持续时间，使用 Controller 可以控制动画的开始和结束以及动画的方向。


### 如何实现一个简单的动画
好了，有了以上基础我们就可以去实现一个简单的动画了。当然这个动画是使用 Flutter SDK 里面已经定义好的动画类，只需要组合一下而已，步骤很简单：

1. 定义一个 Stateful 控件并继承 SingleTickerProviderStateMixin。

    ```dart

    class AnimaState extends StatefulWidget {
        AnimaState({ Key key, this.duration }) : super(key: key);

        final Duration duration;

        @override
        _AnimaStateState createState() => _AnimaStateState();
    }

    class _AnimaState extends State<AnimaState> with SingleTickerProviderStateMixin {
         AnimationController _controller;

        @override
        void initState() {
          super.initState();
          _controller = AnimationController(
            vsync: this, // 指定  SingleTickerProviderStateMixin
            duration: widget.duration, // 指定 动画时间
          );
        }

        @override
        void didUpdateWidget(Foo oldWidget) {
          super.didUpdateWidget(oldWidget);
          _controller.duration = widget.duration;
        }

        @override
        void dispose() {
          _controller.dispose();
          super.dispose();
        }

        @override
        Widget build(BuildContext context) {
          return Container(); // ...
        }
    }
    ```
 2. 接下来就可以使用 Flutter SDK 中定义好的动画控件来定义一个控件了，这里我使用的就是 SizeTransition。Size 表明这个动画效果是使用剪切效果实现的，也就是慢慢的从无到有显示控件。

    ```dart
        Widget _buildAnima() {

            SizeTransition(
                sizeFactor: CurvedAnimation(
                    parent: _controller,
                    curve: Curves.ease,
                ),

                child: Text('Size Transition')
            )

        }
    ```
    其中的 sizeFactor 指定了动画的控制变量以及动画的类型，参数当然可以自己指定。

 3. 接下来就是要控制动画的开始、结束了，这个很简单，在你想要动画开始，以及如何开始的地方，使用 _controller 变量的内置方法即可：

    ```dart
        _controller.forward();   // 正向 开始 动画
        _controller.reverse();   // 反向 开始 动画
        _controller.stop();      // 停止 动画
    ```

好了，这样基本动画已经可以实现了，可以参考官方文档中的各种动画类型，当然最高级的就是自己定义动画了，也就是写 Tween<T> 类了，目前还没学到那里 ... 😃