---
description: 下载源码和编译 ubuntu touch and nexus 6
---
![Ubuntu Touch](https://upload-images.jianshu.io/upload_images/2849484-d59d67bd0cae85eb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

很早就听说 Ubuntu 出移动版了，不过支持的机型还是很少，如果尝试安装的话需要自己改代码、配置文件，因此就没有动手了。不过，最近几天听说 Ubuntu touch 出来了，而且还支持 nexus 4 和 nexus 5，本来想打算一键刷机的，结果一看我是 nexus 6，还没有支持这个机型。不过，我想如果从源码自己编译，应该不会改动太大吧，毕竟4和5都支持了。于是，就开始了下载编译安装....。

## 前言
为了让你尽最大的可能刷机成功，节约你的时间。这里说几点开始之前的注意事项和准备。
1.  开始之前可以看看你的机型是不是满足了源码编译的条件。你可以去 LineageOS 官网看看有没有你的机型，如果有的话，那么你后续尝试起来就很轻松，如果没有也不是不可能，那就真是一个技术活了。
2. 在机型满足的情况下，就要为下载编译做一些准备了。最好有个 Linux 操作系统，比如我就是在 Ubuntu 18.04 下完成的，如果你还不知道 Linux 是什么，那么你估计是迷路了。
3. 关于下载源码。虽然没有原生 Android 源码大，但是下载量也在5个G左右，而且大部分国内都没有镜像吧。因此，你需要很快的网速，如果有科学上网就更完美了。我是用的4G+科学上网大概花了一个小时左右吧。
4. 关于编译的步骤和错误处理。不要被那些看不懂的命令行和不知道是什么的错误提示吓到。不懂也没有关系，有好多我也不太明白，命令的话复制粘贴运行就好，以后在慢慢消化。错误提示的话可以看官方文档对照着解决。说实话，这个文档做的很完善，大家不用担心。

好了，基本上就是这些，如果你准备好了就可以开始编译刷机之旅了。

## 准备
做这种事情，理清头绪很重要。一方面知道自己要做什么，现在做到了哪一步，另一方面出错的话不会乱。因此这里就简单说一些要完成的事情和步骤。

#### 目的
目的很简单，就是在 nexus 6 上安装 Ubuntu Touch 系统。不过，官方没有支持的一键刷机程序。因此就尝试安装 Halium，然后再安装 Ubuntu Touch 文件系统。

#### 步骤
- 首先，下载 Halium 通用源码。也就是和机型没有关系的源代码。
- 然后，下载机型相关的源代码，包括设备驱动，内核、发行商的源码。我的是 nexus 6，moto 生产的，因此下载相应的源码就行了。
- 源代码下载好了，就是修改一些配置，安装一些工具，进行编译、错误处理了。
- 编译成功之后就是安装测试相关镜像了。如果你还不满足那么就去看看源码修改和完善吧。

基本上就是这这几步了，下面就开始命令行操作吧。

### 第一步
准备你的手机和安装开发环境工具，我用的是 ubuntu 18.04，你需要根据你自己的实际情况选择怎么安装。官方给出的文档在[这里](https://docs.halium.org/en/latest/porting/first-steps.html)。

我是 64 位的系统，因此需要运行如下命令添加 i386 支持：
```bash
sudo dpkg --add-architecture i386
```
然后，更新软件列表安装需要用到的依赖：
```bash
sudo apt update
sudo apt install git gnupg flex bison gperf build-essential \
  zip bzr curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
  libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
  libgl1-mesa-dev g++-multilib mingw-w64-i686-dev tofrodos \
  python-markdown libxml2-utils xsltproc zlib1g-dev:i386 schedtool \
  repo liblz4-tool bc lzop imagemagick libncurses5 rsync
```
OK!第一步完成，基本不会出现任何错误，下面进行耗时间的第二步，下载源代码。

### 第二步
根据具体i情况，我的设备是 nexus 6，完美支持 LineageOS 14.1 ，因此选择默认选项就行了。如果你有科学上网工具可以打开，这样下载会快一点，我用的时候下载速度基本上兆，这种体验还是很不错的。

新建，目录然后下载：
```bash
mkdir halium && cd halium
repo init -u https://github.com/Halium/android -b halium-7.1 --depth=1
```

#### 下载设备相关代码
同样和设备有关，官方文档可以看[这里](https://docs.halium.org/en/latest/porting/get-sources.html#adding-your-device-specific-source)。描述的很详细，而且一些配置文件都已经写好了，如果可以的话只需要检查一下，然后直接输入命令下载就完事了。我的机型配置文件很完整，因此直接输命令：
```bash
./halium/devices/setup shamu
```
最后一个是 nexus 6 的 codename。这里主要下载了设备驱动、内核和生产厂商相关的代码。如果，相关的配置文件里面没有你的机型，不急慢慢看文档，一步一步来，你可以的。

### 第三步
代码下载之后就是编译之前的准备工作了，首先初始一下相关的环境变量：
```bash
source build/envsetup.sh
```
然后使用如下命令初始化设备相关的环境变量及参数：
```bash
breakfase shamu
```

#### 修改内核配置
官方说 Halium 用的是 systemd 系统服务工具，因此需要做一些内核配置的修改，还好有工具修改，要不然真的不会：
```bash
git clone https://github.com/mer-hybris/mer-kernel-check
cd mer-kernel-check
./mer_verify_kernel_config <path to kernel configuration>
```

#### 修复挂载点
任然是一些看不太明白的东西，不管了如果你有兴趣可以看[这里](https://docs.halium.org/en/latest/porting/build-sources.html#include-your-device-in-fixup-mountpoints)，如果没有，那就直接在**/halium/hybris-boot/fixup-mountpoints**脚本里面找你机型的 codename，看看有没有，如果有的话恭喜你这一步完成了，如果没有那你就需要一步一步来了。具体步骤看官网吧，这里就不说了。我的是 shamu，所以就不用改什么了。

### 第四步
终于到编译了，首先用如下命令检查工具是否正常：
```bash
mka mkbootimg
```
然后使用如下两个命令构建相关镜像就完事了：
```bash
mka hybris-boot
mka systemimage
```
基本上会让你的CPU持续100%一段时间，编译时间还不算太久，我用了半个小时左右吧。如果你有任何错误可以参考[这里](https://docs.halium.org/en/latest/porting/build-sources.html#documented-errors)。

我只在编译 systemimage 的时候碰到了**Flex locale error**错误，还好解决起来也很简单：
```bash
export USE_HOST_LEX=yes
```
到此为止算是向前进了一大步，下面就是安装刚刚编译好的镜像，具体如何操作，效果如何下次再说说。如果你迫不及待，可以参考[这里](https://docs.halium.org/en/latest/porting/install-build/index.html)，祝大家玩的开心，生活愉快，下期再见!

## 参考
[Halium](https://halium.org/)
[Halium 文档](https://docs.halium.org/en/latest/index.html)
[Ubuntu Touch](https://ubuntu-touch.io/)
