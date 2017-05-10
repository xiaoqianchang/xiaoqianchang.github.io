title: 在Windows下搭建React Native Android开发环境
date: 2017-02-25 20:31:36
categories: ReactNative
tags: 
  - ReactNative
  - 环境搭建	
---

## 安装git for windows

在[这里](https://git-for-windows.github.io/)下载安装，`安装过程中注意选择"Run Git from Windows Command Prompt"。`

## 安装Python

从[官网](https://www.python.org/downloads/release/python-2710/)下载并安装python 2.7.x（3.x版本不行）

<!--more-->

## 安装node.js

从[官网](https://nodejs.org/en/)下载node.js的官方4.1版本或更高版本。

建议设置npm镜像以加速后面的过程（或使用科学上网工具）。
``` bash
npm config set registry https://registry.npm.taobao.org --global

npm config set disturl https://npm.taobao.org/dist --global
```

## 安装react-native命令行工具

``` bash
npm install -g react-native-cli
```

安装react native模块以及相关node模块`npm install  --save react-native`

## 创建项目

进入你的工作目录，运行

	react-native init MyProject

并耐心等待数（或数十）分钟。

## 运行packager

	react-native start

可以用浏览器访问http://localhost:8081/index.android.bundle?platform=android 看看是否可以看到打包后的脚本（看到很长的js代码就对了）。第一次访问通常需要十几秒，并且在packager的命令行可以看到形如[====]的进度条。

如果你遇到了`ERROR Watcher took too long to load`的报错，请尝试修改`node_modules/react-native/packager/react-packager/src/FileWatcher/index.js`，将其中的MAX_WAIT_TIME 从25000改为更大的值（单位是毫秒）

## 运行模拟器

推荐使用Genymotion。
如果有真机，可以不必运行模拟器，要配置好驱动，使得adb devices可以看到对应的设备。

## 安卓运行

`保持packager开启`，另外打开一个命令行窗口，然后在工程目录下运行

	react-native run-android

首次运行需要等待数分钟并从网上下载gradle依赖。（这个过程屏幕上可能出现很多小数点，表示下载进度。这个时间可能耗时很久，也可能会不停报错链接超时、连接中断等等——取决于你的网络状况和墙的不特定阻断。总之要顺利下载，请使用稳定有效的科学上网工具。）

运行完毕后可以在模拟器或真机上看到应用自动启动了。

如果apk安装运行出现报错，`请检查上文中安装SDK的环节里所有依赖是否都已装全，platform-tools是否已经设到了PATH环境变量中，运行adb devices能否看到设备`。

至此，`应该能看到APP红屏报错，这是正常的，我们还需要让app能够正确访问pc端的packager服务`。

摇晃设备或按Menu键（Bluestacks模拟器按键盘上的菜单键，通常在右Ctrl的左边 或者左Windows键旁边），可以打开调试菜单，点击Dev Settings，选Debug server host for device，输入你的`正在运行packager的那台电脑的局域网IP加:8081`（同时要保证手机和电脑在同一网段，且没有防火墙阻拦），再按back键返回，再按Menu键，在调试菜单中选择Reload JS，就应该可以看到运行的结果了。

如果真实设备白屏但没有弹出任何报错，可以在安全中心里看看是不是应用的“悬浮窗”的权限被禁止了。

## 安卓调试

打开Chrome，访问 http://localhost:8081/debugger-ui， 应当能看到一个页面。按F12打开开发者菜单。

在模拟器或真机菜单中选择Debug JS，即可开始调试。

