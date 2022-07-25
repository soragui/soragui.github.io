---
layout: post
image: assets/images/flutter.png
comments: true
author: sal
categories: flutter
---
>接触Flutter已经一个月了，自认为已经入门了，所以就来写一写自己的学习笔记，希望可以帮助到那些对Flutter感兴趣的人。

### 什么是Flutter
在学习一门技术之前，总是需要先了解一下这门技术是什么，这对于提高学习效率是有很大帮助的。首先，你可以联系现有的知识来比较两者之间的区别和联系，然后了解新技术是为了解决什么问题、使用的什么新的方法，这样就可以规划出一条清晰的学习路线，也不至于迷茫。

好了，现在来说一说我眼中的Flutter。不过首先还是要说一下我的基础，计算机专业出身的我，这么多年过去了并没有怎么写代码，但任然对程序员有着一种莫名的期待，总感觉自己可以写出改变世界的代码，这也是接触Flutter的原因之一。不过我写的最多的就是弄了两个Android应用，一句话概括就是页面少而简单，代码基本是没有什么逻辑可言。所以总的的来说就一句话，我就是一个编程小白，有着成为大神的梦想...

现在开始说一说我对Flutter的理解了，简单来说Flutter就是一个由Google跨平台的应用开发框架，但其主要应用的还是移动开发，就像是Facebook开发的RN一样，为了解决Android和iOS程序员开发的痛苦。说实话一开始我对Flutter并没有什么兴趣，但知道这个是Google开发的，瞬间动力就不一样了。

小白的我并没有接触过RN，所以他们两的区别和联系并不知道，只是看过别人对他们两的评价，最终得出结论Flutter无论从任何地方都要比RN要强，而且开发出来的效果和原生态并没有什么区别...我想应该比原生态开发要好，毕竟这个可以跨平台。

### 如何开始
现在可以开始配置开发环境了，既然是跨平台的，那么就可以在windows macos linux中开发，不过我建议还是要在macOS中开发，因为Windows 和 Linux都没法编译iOS应用，那么跨平台也就失去了意义，不过还是可以学习的...

我也是屌丝没有钱苹果买不起，因此只能装一个黑苹果了，开发起来完全没有问题。关于安装配置官网上也有很清楚的说明，如果你英文不错，可以参考[这里](https://flutter.io/get-started/install/),英语不过关的同学可以参考[这里](https://flutterchina.club/get-started/install/)。下面我就来简单说一下我是如何在macOS上安装使用的。

#### Flutter SDK 安装
首先就是Flutter SDK的安装，首先确定Flutter存放的目录，进入那个目录，然后使用如下命令克隆：
```zsh
git clone -b beta https://github.com/flutter/flutter.git
```
下载好之后需要把bin目录添加到PATH环境变量里面，这样就可以很方便的使用flutter命令来进行工程管理了。经常使用到的命令有如下几种：
```zsh
flutter create  # 建立新的 Flutter 工程
flutter run     # 运行 Flutter 程序

flutter doctor  # 检测安装的工具情况
flutter upgrade # 更新 Flutter 
flutter channel # 列出或更改 Flutter 开发发布 的分支
flutter package # Flutter package 操作
```
设置好环境变量之后，你就可以在终端运行一下**flutter upgrade**来更新了，会下载最新的flutter引擎和Dart SDK，这些都是编译flutter工程所必须的。

如果你是在中国，或者没有代理你需要设置几个环境变量来加速flutter package的下载速度：
```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```
基本是flutter SDK就已经配置完成，下一步就是IDE了。

#### IDE 的选择
当然大部分人都会选择Android studio来进行开发，毕竟这个看起来专业一点，不过我个人觉得刚开始还是可以考虑一下visual studio code的，因为这个可以让我们熟悉一下命令行相关的操作，我相信大部分同学在学习Java的时候也不是一开始就用IDE的，都是在编辑器加命令行了，并且现在这个VS Code功能很强大了，用起来一点也不比AS要差。不管你用哪一个IDE，安装两个插件还是有必要的：flutter和Dart。

在AS中只需要手动安装flutter就可以了，因为Dart会自动作为依赖安装，VS Code中直接搜索安装就行了。

如果需要进行iOS开发，当然还需要Xcode和一些其他依赖工具，其中一个就是在iPhone上运行应用的部署工具，可以[homebrew](https://brew.sh)通过如下命令进行安装：
```zsh
brew update
brew install --HEAD libimobiledevice
brew install ideviceinstaller ios-deploy cocoapods
pod setup
```

应该就是这么多了吧，写的有点乱。

### 参考及学习资源

官方文档我就不说了，这应该是我们入门必须要看的，而且会一直看下去，毕竟这里有各种package和类的说明、参数及使用情况。

[awesome-flutter](https://github.com/Solido/awesome-flutter)flutter的开源应用，框架及课程资源。
[awesome-dart](https://github.com/yissachar/awesome-dart)关于Dart语言的各种资源
[闲鱼技术](https://www.yuque.com/xytech/flutter)闲鱼技术团队研究flutter很勤快，也很深入
 另外，简书和掘金上的flutter博客也有不少，干货很多。最后希望flutter发展越来越好，学习和探索flutter的人越来越多....
