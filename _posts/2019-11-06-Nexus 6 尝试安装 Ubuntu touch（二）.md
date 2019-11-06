---
description: 修改内核配置和挂载点
---

![Ubuntu Touch](https://upload-images.jianshu.io/upload_images/2849484-d59d67bd0cae85eb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

昨天粗略的看了一下相关的文档，很兴奋结果就是并没有仔细的阅读和了解，结果就导致很多东西都说的有失偏颇甚至有些错误和误导。因此，今天的任务就是把昨天错误的地方指正，说仔细一点。其次，就是分享一下第一次安装成功的喜悦心情，并附上相关源码和镜像以供大家使用。

## 说明
首先说一下 Halium 和 Ubuntu Touch 的关系。Halium 是一个硬件抽象层，不过具体是如何实现以及如何组织的这里就不说了，因为还没有彻底理解怕误人子弟，感兴趣的可以到[这里](https://halium.org/)了解更多。而 Ubuntu Touch 真的就是一个移动版的操作系统，因为暂时还没有 Nexus 6 的刷机。因此只能通过 Halium 来安装 Ubuntu Touch 官方出的 **rootfs** 来体验一下了。

下面简单说一下今天的任务，昨天的步骤有一点过于简单，其中内核配置和挂载点修改的地方最容易出错没有仔细说，所以今天就来看看到底怎么修复：
   1. 首先说明一下怎么配置设备相关代码的下载源问题。虽然Halium已经给出了相关配置，不过或许不是最新的源配置，检查一下总是放心一点。
2. 下载好内核代码和驱动代码之后就是修改内核配置文件了。一般都是 **codename_defconfig** 文件，里面是一些关于编译哪些组件的配置，因此为了满足 Halium 的需求做相应的修改。
3. 挂载点修复问题。应该是有关文件系统如何挂载的问题吧，如果不正确就启动不了。
4. 初步尝试 Halium 的 rootfs。编译成功之后刷镜像，然后启动并配置相关IP，ssh 登录后看看相关效果。

## 配置源码仓库
这里就是配置设备相关源码的仓库，之后下载的时候就是从你配置的地方拉取过来。配置文件的路径是在：
```
halium/devices/manifests/[manufacturer]_[device].xml
```
如果你找不到你设备的配置文件，就新建一个，我的配置文件是 **motorola_shamu.xml**，其内容如下，当然这是我修改之后的：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
    <remote name="lyo"
            fetch="https://github.com/LineageOS"
            revision="lineage-17.0" />

    <project path="device/moto/shamu" name="android_device_moto_shamu" remote="los" />

    <project path="kernel/moto/shamu" name="android_kernel_moto_shamu" remote="los" />

    <project path="vendor/moto" name="proprietary_vendor_motorola" remote="them" />
</manifest>
```
这里的 **project** 就是一个配置项目，其中 path 是源码下载的目录，name 是远程仓库的名字，而 remote 则是远程仓库的地址别名。你可以看[这里](https://docs.halium.org/en/latest/porting/get-sources.html#halium-7-1)是官方支持的远程仓库，你可以使用 <remote /> 来添加自己的远程仓库。具体属性就不说了，参考官方文档就可以了。OK，到此位置已经配置好远程代码仓库了，下面就来修改一下内核配置文件吧。

## 修改内核配置
昨天的脚步命令只是给出了一些配置建议，并没有自动修复相关的配置，其实这些配置我也不太理解是什么作用，反正先按照建议修改一下呗，以后再慢慢探索了。以我的配置文件为列，起目录在：
```
kernel/moto/shamu/arch/arm/configs/shamu_defconfig
```
然后使用如下命令查看输出结果，也就是一些警告和错误提示：
```bash
./mer-kernel-check/mer_verify_kernel_config kernel/moto/shamu/arch/arm/configs/shamu_defconfig
```
其输出结果如下，当然这是我修改之后再次运行的结果：
![内核配置检测](https://upload-images.jianshu.io/upload_images/2849484-471e88636e3dec83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中每一项都以换行分开了，就拿第一项做一个例子吧！最前面是一个大写的 WARNING，表示修不修改问题不大，然后就是第二行表示没有设置任何的值，第三行表示允许设置的值，第四行就是注释了，表明这个配置的是什么以及一些建议。

#### 如何修改
这个很简单，用文档编辑器打开，然后查找相应的配置选项进行相应的修改就行了。如果你怕麻烦，只需要修改 ERROR 项就行了。你可以使用 grep 命令来看看有那些是必须要修改的项目。

最后，有两个配置没有写到检查里面，CONFIG_IKCONFIG 和 CONFIG_IKCONFIG_PROC ，这两个都要设置成 **y**，否则就无法启动了。可以参考官方文档查看。

## 挂载点修复
同样的以我的设备为例，挂载点修复脚步的位置为：
```bash
halium/hybris-boot/fixup-mountpoints
```
找到这个文件，打开后搜索 shamu ，看到有一些修复项目，不过还是要检查一下是不是齐全。下图是我修改后的结果：
![挂载点](https://upload-images.jianshu.io/upload_images/2849484-7d36436a4ef31037.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对比的话首先找到，设备相关的挂载文件，我的位置在：
```
device/moto/shamu/fstab.shamu
```
然后查看对比官方的修复脚本有没有漏掉的，所有的挂再点类型不是 auto emmc swap 的都需要修改，具体修改的值可以使用如下方面获取：

1. 使用 adb shell 进入，并得到 root 权限。
2. 使用 readlink -f [src] 获取挂载点的返回值

其中 src 就是你在 fstab 文件里面看到的第一列的值。获取了修复值之后就可以添加到相应的位置上去就行了。

## 结果
一切就绪之后就是编译了，成功之后下载两个镜像文件加文件系统，这里我下载的是 halium 官方文件系统，启动之后一直停留在开机画面上，只能通过 ssh 进入系统，步骤如下：
1.  Halium 默认启用了USB网络连接，查看手机IP即可。
2. 更改电脑端相应接口的IP，让他们在同一个局域网中。
3. 使用 ssh root@ip 登录即可。

具体步骤可以参考[官方文档](https://docs.halium.org/en/latest/porting/debug-build/early-init.html)，这里就不详细说了，直接上图。
![查看手机IP](https://upload-images.jianshu.io/upload_images/2849484-fbbb7527c1f6350e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![设置电脑端IP](https://upload-images.jianshu.io/upload_images/2849484-003f4dd472b1c980.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![登录成功](https://upload-images.jianshu.io/upload_images/2849484-ceab890251f60477.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，今天就到这里，感觉还有好多东西没有描述清楚，不急以后慢慢探索、尝试。祝大家生活工作愉快，下期再见。
