---
layout: post
author: sal
image: assets/images/flutter.png
comments: true
---
> 如何定制自己的 Floating Action Button

基于上一次的Hello Widget，这次我们就来说说如何定制自己的FloatingActionButton。其实大家也不要想象的那么高大尚，或许是我想的太简单了，这里我只是利用现有的Widget，而没有自己去真正的绘制出一个Widget。好了不说那么多了开始吧！

### 原始Button
首先我们来看看最初的FAB是什么样子的，其代码如下：
```dart
floatingActionButton: new FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: new Icon(Icons.add),
      )
```
floatingActionButton 是作为参数传递给 Scaffold 的，这个只是一个参数名，我们需要看看它的类型是什么，不过我的第一直觉它就是一个 FloatingActionButton 类，然而我错了，我们可以用 `command + 鼠标左键` 来看看它的类型如下：
<img src='/assets/images/floatingActionButton/floatingactionbar.png' alt='Scaffold floatingActionButton Type'>

好了，下面开始写属于我们自己的FloatingActionButton,首先新建一个文件命名为 `fancy_button.dart`,然后写下如下代码：
```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

class FancyButton extends StatelessWidget {

  final GestureTapCallback onPressed;

  FancyButton({@required this.onPressed});

  @override
    Widget build(BuildContext context) {
      // TODO: implement build
      return null;
    }
 
} 
```
其中开头import了两个package，可以看出定义了一个继承自StatelessWidget类的FancyButton，因为这个Button没有状态改变，定义了一个 onPressed 属性，用来作为点击事件的回调,然后就是重写 build 函数，直接 return 一个你想要的 widget。基本框架已经搭建完成，下面就修改 main.dart 文件里面的东西，首先 import 这个dart文件，然后把之前的 floatingActionButton 修改为如下：
```dart
floatingActionButton: new FancyButton(
        onPressed: _incrementCounter, 
      ),
```
你现在就可以 flutter run 看看效果，不过会报错，因为我们在FancyButton中返回的是 null,下面就从最简单的开始。

写一个Text展示一个文字提示：
```dart
@override
    Widget build(BuildContext context) {
      // TODO: implement build
      return Text('Add Number');
    }
```
很简单，就是返回了一个Text，连TextStyle都没有，当然实际显示效果也很简单，可以用在终端 flutter run ，之后修改的只需要直接输入 r 进行 reloaded 就行了，很方便。

一个Text看起来实在是很平淡，所以需要一点阴影效果，让Button看起来更有层次感，而在Material UI中这是实现起来很简单，就是Z轴的高度，那么我们可以添加一个 Material 来实现：
```dart
return Material(
        elevation: 4.0, 
        child: Text('Add Number'),
      );
```
其中 elevation 就是用来设置Z轴的高度的，不过这个看起来很紧凑，很方，那么可以通过 borderRidus 来设置圆角：
```dart
return Material(
        elevation: 4.0, 
        borderRadius: BorderRadius.all(Radius.circular(10.0)),
        child: Text('Add Number'),
      );
```
如果需要修改背景色，可以通过 color 属性来设置，不过这种看起来太紧凑了，因为没有padding, 这时需要在 Text 外面包一层 Padding，来设置 子控件和父控件 的边界距离：
```dart
child: Padding(
          
          padding: EdgeInsets.only(left: 8.0, right: 8.0, top: 4.0, bottom: 4.0),
          
          child: Text('Add Number'),

        )
```
padding 属性传入的是 EdgeInsets ，其构造函数 有 all , only  和 fromLTRB等，这里使用的是 only，可以自行按需要设置上下左右的 padding, 如果是 all 的话，上下左右都一样。而 fromLTRB 是按照 左上右下的顺序依次设置，必须都设置。有了 padding 这下看起来都舒服多了。

不过等一下，到目前为止，都没有添加点击事件，点一下都没有反应啊。所以下面就来添加一个点击控件，在 Flutter 中最原始的就是GestureDetector了，里面定义了好多手势回调函数，包括点击、按、长按、滑动等等，反正这个很丰富，另外就是 InkWell，带波纹的点击控件，还有就是 FlatButton、RaisedButton。

这里我们就是用 GestureDetector 了，如下所示：
```dart
child: Padding(
          
  padding: EdgeInsets.only(left: 8.0, right: 8.0, top: 4.0, bottom: 4.0),
    
  child: GestureDetector(

       onTap: onPressed,
       child: Text('Add Number'),

     ) 
   )
```
我们把 FancyButton 中定义的 onPressed 传入其 onTap 属性，然后 reloaded 一下，就可以实现点击事件了，和刚开始的效果一样，这就是 Flutter 中回调函数最精彩的地方。 

不过，还没有波纹效果，这样用户体验就不是很好了，MD中说要有用户回馈，这样用户体验也就上来了，而 GestureDetector 中并没有相应的属性，所以我们就专用 InkWell 了，并设置了 ink 的相关颜色：
```dart
child: InkWell(
            
            splashColor: Colors.amberAccent,
            onTap: onPressed,
            child: Text('Add Number'),

          )
```
不过这个看起来很奇怪，因为只有文字被 InkWell 包围了，所以我们要把 InkWell 提升到最控件树的最顶端.
```dart
Material(
          elevation: 4.0, 
          borderRadius: BorderRadius.all(Radius.circular(10.0)),
          child: InkWell (
            
            onTap: onPressed,

            child: Padding(
            
              padding: EdgeInsets.only(left: 8.0, right: 8.0, top: 4.0, bottom: 4.0),
              
              child:  Text('Add Number'),

            ) 
        ),
      );
```
这样开起来就舒服多了，注意 如果没有设置 Tap 回调事件，那么同样是没有波纹效果的。

如果再加一个图标那就更完美了，一般都是一个图标，加一个文字描述，水平排列。而 Flutter 布局中 用 Row 来进行水平布局，所以把之前的 Text 修改为如下所示：
```dart
Row(
 
    children: <Widget>[

      Icon(Icons.add),

      Text('Add Number'),
      
    ],

  ) 
```
如果 reloaded 一下，就会发现很这个 floatingActionButton 很奇怪，因为没有设置 mainAxisSize 属性，这个属性控制 Row 控件在水平方向的大小，默认是 max,我们这里只需要设置为 min 就可了，添加如下：
```dart
mainAxisSize: MainAxisSize.min,
```
这里的 main 的意思就是主轴，如果是 Row 那就是水平方向，如果是Column 那就是垂直方向.

嗯，感觉很😊...今天就先到这里吧，我也才刚开始学习Flutter，一边学一边记笔记，今后还是要勤快一点了...