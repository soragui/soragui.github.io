---
layout: post
image: assets/images/mathtype.jpg
comments: true
author: sal
---
Mathtype 是由Design Science公司设计的一个可以在Word、PowerPoint等文字处理和演示程序中编辑的科学公式编辑器。
支持TeX和LaTeX，以及国际标准MathML，并能够把公式转化为支持Web的各种图形（GIF等），也支持WMF和EPS输出。

不过在中国大家还是熟悉在写论文的时候用到此编辑器，要想让你的论文看起来高大上一大堆公式那是必不可少的，因此本文就来叫你如何优雅的在Office中使用MT编辑各种数学公式。

## 安装 MathType
首先第一步当然是按照MathType了，不过在这里就不介绍如何安装Office了，相信大家都已经完成这一步了。直接跳到文章末尾[下载](#下载)MathType，下载完成然后按照如下步骤进行安装：

1. 双击下载的exe文件进行安装
2. 点击接受许可证然后下一步
3. 点击第二个选项然后填入如下注册信息：

|             |                      |
| ----------- | -------------------- |
| Fname       | abc                  |
| Lname       | def                  |
| Orgnation   | Nill                 |
| Product Key | MTWE691-011084-r0xyl |

4.邮箱的话随便，然后下一步完成安装。

## 安装Office插件
现在可以打开你的Word看看工具栏里面有没有MathType这一项：

![](/assets/images/mathtype/mathplugin.png)

如图所示，如果有的话说明完全安装成功，虽然都是英文但经常用的也就第一项。如果没有也不要那么担心，下面就来教你如何轻松添加。

- 首先确认你Office的安装位置，以及是32位的还是64位的，如果你的系统本身就是32位的那么不用担心，你的Office就是32位的。如果你的系统是64位的，那么看看你Office的安装路径，如果是在*Program Files*目录下就是64位的，否则就是32位的。
- 然后找到一个叫 Office ** 的目录，这里的 ** 代表 Office 版本，比如我的是 Office 16，我的完整路径就是：*C:\Program Files (x86)\Microsoft Office\root\Office16*，里面有一个 STARTUP 目录，如果没有就直接新建一个。好的保持打开不要动。
- 新建一个资源管理器窗口，定位到MathType的安装目录，然后找到MathPage目录，里面有一个32位和64位目录，就是刚刚查看的Office位相对应的，打开相应的目录，把里面的文件拷贝到刚刚的STARTUP目录里面。再然后，到 Office Support 目录里面，拷贝 *MathType Commands 6 For Word.dotm* 文件到 STARTUP目录里面。就此打工告成了。

现在可以重启Word看看有没有MathType那一栏了，如果还是没有欢迎留言。祝大家生活愉快，论文越写越好。

## 下载
MathType [下载链接](https://pan.baidu.com/s/1U1skjDPdhaTX77zyDuJ24g) 提取码：2exx
