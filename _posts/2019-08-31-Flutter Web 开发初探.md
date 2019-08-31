> Flutter 作为一个跨平台UI设计工具，在iOS和Android上的表现已经有目共睹，那么今天就来看看如何用Flutter来开发简单高效的Web程序吧。

## 基础
因为有了*dart2js*工具，所以Flutter开发web应用成为可能，Dart用来设计UI还是很不错、很方便的。因此如果你设计的网页没有太多的后台交互，注重动画UI，我觉得用这个工具还是很不错的。至少对于我这种还不熟悉CSS和JS的同学来说是一个很好的解决方案。

因为目前Flutter Web还是在review阶段因此，它和主流Flutter框架是独立的，所以需要独立下载工具和框架。而且，开发的package也是独立的，也就是说在pub里面的package需要使用有web标签的package。

## 安装
首先安装 [webdev package](https://pub.dartlang.org/packages/webdev) ，这个包提供了开发web的构建工具。
```bash
flutter pub global activate webdev
```
![Install](/assets/images/flutterweb/install.png)
不过，首先你得把 .pub-cache/bin 目录在你的系统path中，默认情况下此目录在你之前安装的flutter的根目录里面。比如，我自己把flutter安装在了Library里面，那么其路径就是**$HOME/Library/flutter/.pub-cache/bin**。

## 使用 VSCode 开发
- [配置](https://flutter.dev/docs/get-started/editor?tab=vscode)VSCode
- 配置VS Code中的Flutter SDK路径
- 在命令面板中运行 Flutter: New Web Project
- 然后等待工程创建完成，就可以在Debug 面板中开始调试了
- VS Code 会自动使用之前安装的 webdev 命令来构建运行这个应用了。

可以看一下，初步运行的情况和代码进行对比：
![Install](/assets/images/flutterweb/helloworld.png)

目录结构如下：

![Install](/assets/images/flutterweb/prostr.png)

从中可以看出，和普通Flutter结构相比，多了一个web目录，在这里你可以写普通的页面，然后和相应的JS文件相联系就行了。

比如在 index.html 中有如下代码：
```html
<head>
  <meta charset="UTF-8">
  <title></title>
  <script defer src="main.dart.js" type="application/javascript"></script>
</head>
```
其中加载了 *main.dart.js*，这个文件就是lib中main.dart编译成js之后的文件。

## 一些限制
因为web框架还处在开发阶段，还有好多东西需要完善，因此现阶段还是处于体验阶段。浏览器中完全支持Flutter的所有API和功能。但是，在此预览期间，有许多例外情况：

- flutter_web还没有插件系统。暂时，提供对dart：html，dart：js，dart：svg，dart：indexed_db以及其他允许您访问绝大多数浏览器API的Web库的访问权限。但是，期望这些库将被不同的插件API替换。
- 并非所有Flutter API都在Flutter for web上实现。
绩效工作才刚刚开始。 Flutter for web生成的代码可能运行缓慢，或者显示重要的UI“jank”。
- 目前，桌面UI交互并不完全，因此使用flutter_web构建的UI可能会像移动应用程序一样，即使在桌面浏览器上运行也是如此。
- 开发工作流程目前仅适用于Chrome。