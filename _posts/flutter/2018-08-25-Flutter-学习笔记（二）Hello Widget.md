---
layout: post
author: sal
categories: flutter
description: flutter 学习笔记 hello world widget 工程目录详细解析
image: assets/images/flutter.png
comments: true
---

> Flutter 中的一切都可以视为 Widget，好吧我还是理解的不太深入，不过解释一个简单的Widget还是可以了，那么下面我们就来看看Flutter中的Hello Widget 是什么样子的吧！

### 创建Flutter Project
新建Flutter Project(aka FP)，有两种方法，一种是命令行模式，另外一种就是在AS中一路Next。我这里选择用命令行模式+VS CODE，因为只是写一个简单的Widget，这样看起来简单高效加专业。

打开VS Code，然后 Ctrl+` 打开命令行窗口，CD到你想要的目录，然后用如下命令创建一个工程：
```zsh
flutter create hellowidget
```
用VS Code打开工程后，定位到main.dart文件，对于刚开始接触Flutter的小白来说，虽然只有简单的几个类，几个方法，但还是看着有点头晕，不管了先使用如下命令运行：
```zsh
flutter run
```
此命令默认在Debug模式下编译安装运行，不过你要确保有物理机或模拟器连接，可以使用如下命令查看：
```zsh
flutter devices
```

### main.dart 分析
纯小白的动作，这么简单的文件还要分析，没办法，为了以后还是要理清一下思路的。

首先 第一行发现了一个 import 语句，这应该就是flutter导入package的方式，发现导入了 **flutter/material.dart** 文件，这个文件应该是包含了flutter中最基础的方法和widget了。

然后发现了 main 函数的入口如下所示：
```dart
void main() => runApp(new MyApp())
```
如果之前没有接触过Dart语言，或者类似的语言，那么这个语法看起来很不友好，那么我们就把它还原把，如下：
```dart
void main() {

    return runApp(new MyApp());

}
```
修改之后，就可以去刚才运行程序的命令行输入字母 r 看看效果，是一样的。这里字母 r 表示进行 Hot reload ，这也是flutter最主要的特性之一了。

好了，继续往下看，发现一个 MyApp 类，并继承了 StatelessWidget ，里面只重写了一个 build方法，该方法返回了一个MaterialApp实例，这就是你看到的整个界面的最顶层widget，然后看看往里面传递的参数：
```dart
return MaterialApp{
    title: 'Flutter Demo'
    theme: ThemeData {
        primarySwatch: Colors.blue,
    },
    home: new MyHomePage(
        title: 'Flutter Demo Home Page'
    ),
}
```
不熟悉Dart语言函数传参方式的同学，看着可能有点晕，没关系我一开始也是，不过后来看多了，觉得这样传参很清晰易懂。

好了，一共有三个参数，都很明显：title、theme和home，在home里面又实例了一个MyHomePage类并传入了一个 title 参数，那么我们再往下看 MyHomePage 的实现。

可以看到，MyHomePage 继承自StatefulWidget，加上前面说的 StatelessWidget，这两个Widget应该是flutter中的核心Widget，不过小白对此理解的也不是很深入，所以就不多说了。继续看MyHomePage，里面有一个属性 title 和一个方法：
```
_MyHomePageState createState() => _MyHomePageState();
```
此方法的名为 createState() ，返回值为 _MyHomePageState,函数体为 _MyHomePageState();

下面来看看 _MyHomePageState 的具体实现，这个应该是整个程序最主要的地面，毕竟主界面和事件监听都来自这里。

此类中定义一个属性：
```dart
int _counter = 0;
```
在Dart中，变量名前加 _ 表示私有成员，类似 private 修饰符，
下面就是定义了一个私有的方法：
```dart
void _incrementCounter() {

    setState( () {
        _counter ++;
    })

}
```
在此方法中又调用了一个 setState() 方法，此方法是 StatefulWidget 用来更新时所调用的方法，调用该方法之后 重写的 build 函数会再次执行，重绘整个界面这就是flutter更新界面的简单流程了。

setState() 方法传入的是一个 函数参数 如下：
```dart
() {
    _counter ++;
}
```
如果参照之前 main 函数的写法，可以写成如下形式：
```
setState( () => _counter++ )
```
在这里你还可以把 _incrementCounter 方法写出如下形式：
```dart
void _incrementCounter() =>
    setState(() => _counter ++); 
```
然后下面就是绘制整个界面的 build 函数了，依然只有一个 return 语句，这一次返回了一个 Scaffold，然后有三个参数：title 、body和floatingActionButton，其中 title 是一个 Text() 控件，body如下：
```dart
body: Center (
    child: Column (

        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[

            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.display1,
            ),
        ]
    )
)
```
可以很清楚的看到body中控件的逻辑结构，首先是一个 Center Widget，然后有一个 child Wiget 是 Column，其中有一个参数为 mainAxisAlignment 是主轴的对齐方式，因为这里是列控件，主轴也就是垂直方向上的对齐方式为中心对齐，还有一个数组参数，成员为 Widget，这里包含了两个 Text，这就是你在界面上看到的两个Text了。

最后就是一个 floatingActionButton 参数，这里传入了一个 FloatingActionButton() Widget，注意这里可以传入任意的Widget，所以自定义一个 floatingActionButton 很简单。FloatingActionButton 参数如下：
```dart
FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: new Icon(Icons.add),
      )
```
onPressed 表示点击事件，其原型是一个 VoidCallback 回调函数，所以这一把 _incrementCounter 函数作为参数传递进出，tooltip 是提示，长按是会出现，那么child就是子Widget了，这里是一个 + 号图标。

总算是介绍完了，对flutter的一些概念理解的还不是很熟悉，所以有点乱了。

### 参考资料
[create project](https://flutter.io/get-started/test-drive/#terminal)

[stateful and stateless widget](https://flutterbyexample.com/flutter-widgets#stateless-and-statefulwidgets)