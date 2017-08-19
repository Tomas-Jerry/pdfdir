# pdfdir —— PDF导航书签添加工具

[![Build Status](https://travis-ci.org/chroming/pdfdir.svg?branch=master)](https://travis-ci.org/chroming/pdfdir)

根据已有的目录文本为你的PDF自动生成导航书签。

![](https://user-images.githubusercontent.com/9926275/29131441-7fb95754-7d5f-11e7-8ebe-78b989ff1984.png)

*此项目实现逻辑深受 https://github.com/ifnoelse/pdf-bookmark 项目影响。*

## 软件功能

根据网上或PDF中已有的目录内容自动将导航书签插入PDF文件中。

适用于以下场景：

1. 扫描版电子书籍无导航书签；
2. 文字版电子文档无导航书签但PDF中有目录。

## 下载

Windows:

[下载地址](https://github.com/chroming/pdfdir/releases)

Macos/Linux:

请暂时使用源码运行。源码运行方式见后文。

## 基本用法

### 使用

+ 选择文件：在 "PDF文件路径" 文本框中填入pdf文件路径（形如D:/1.png）或点击 "打开" 按钮通过文件管理器选择所需的pdf文件。
+ 目录文本：将目录文本粘贴到“目录文本”框中，如何获取目录文本将在后文说明。
+ 偏移页：指实际页数与文档内容下标页码的差值，如：第一章实际在pdf的第5页，但此页的页面下标页码为第1页，则偏移页为4 。
+ 写入目录：前三个均填好之后就可以点击 "写入目录" 自动将目录写入pdf拷贝中，拷贝文件自动命名为 "原文件名_new.pdf"。

###  获取目录文本

目录文本是类似以下形式的文本内容：

```
中译版序言
致中国读者
作者来信
前言
第1章社会心理学导论2
第一编社会思维
第2章社会中的自我32
.....................
结语605
参考文献606
```

此文本内容一般来源于网上书店（如亚马逊）或图书介绍网站（如豆瓣读书）。图书的介绍中一般会列出该书的目录文本，如亚马逊的在 *商品描述--目录* 下。注意：自动生成的目录完全依赖于目录文本，如果此文本有问题则生成的目录也会有问题。

### 已知问题

+ 一般图书非正文部分（如序言，目录等）没有标页码或使用另一套页码标记，本程序将这些目录默认链接到第一页，如需修正这些链接可手动修改。
+ 有些正文中的目录没有标页码，程序会将该条目录链接到上一个有页码的页面。
+ 程序目前还不处理子目录结构，所有导航书签均放在第一级。（处理子目录功能将在下一版实现）

## 其他

### 通过源码运行

运行源码所需环境：

+ Python2/3 均可，推荐Python3
+ PyQt5
+ PyPDF2

安装Python3:

https://www.python.org/downloads/

安装PyQt5, PyPDF2:

`pip install pyqt5`

`pip install pypdf2`


环境装好之后进入源码目录，运行以下命令：

`python run_gui.py`

如果不需要界面的话:

`python run.py`

### 打包源码

如果你想在本机打包此程序：

本程序使用 Pyinstaller 打包，当然你也可以选择自己喜欢的打包工具进行打包。

打包时注意：如果你修改过 `main.ui` 文件则需在打包前运行以下命令将ui文件转换为py文件：

`pyinstaller -i 0 ./gui/main.ui -o ./gui/main_ui.py`

### 目录文本格式

目前通过以下格式处理目录文本：

*标题+页数+换行符*

所有在一行的都被认为是一条目录。页数通过正则`(\d*$)`匹配（匹配文本结尾处的所有数字），如果匹配不到则默认为第一页或上一条目录的页数。
