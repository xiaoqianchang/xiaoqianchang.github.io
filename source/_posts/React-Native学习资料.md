title: React Native学习资料
date: 2017-02-26 16:40:09
categories: ReactNative
tags:
  - ReactNative
  - 学习资料	
---

本文记录React Native 从环境搭建、签名打包等等一些资料。

## React Native进行签名打包成Apk

对于项目中不存在react.gradle文件的项目打包

首先命令切换到该react native项目的主目录，然后运行以下的命令，生成assets文件夹

	mkdir -p android/app/src/main/assets

紧接着运行以下命令，进行生成inde.android.bundle文件

``` bash
react-native bundle --platform android --dev false --entry-file index.android.js \
  --bundle-output android/app/src/main/assets/index.android.bundle \
  --assets-dest android/app/src/main/res/
```

<!--more-->

最后运行之前的命令，进行代码和资源文件打包，生成的带有签名的apk还是在上面的目录中。

	cd android && ./gradlew assembleRelease

注意：这时有可能不成功，报红色的错误
``` bash
* What went wrong:
Execution failed for task ':app:processReleaseResources'.
> com.android.ide.common.process.ProcessException: org.gradle.process.internal.ExecException: Process 'command 'D:\software\android\sdk\build-tools\23.0.1\aapt.exe'' finished with non-zero exit value 1
```

这可能是由于在生成res时图片资源的名称过长，超过了windows的限制导致的。

运行Apk

	cd android && ./gradlew installRelease

## React Native库版本升级(Upgrading)与降级讲解

- 更新React Native项目依赖包版本

查看本地的React Native的版本

	react-native --version

查询react-native的npm包得最新版本(react native的npm包的地址为: https://www.npmjs.com/package/react-native )，或者采用命令`npm info react-native`进行查看。

在修改package.json文件后，我们需要命令行切换到项目的主文件夹重新执行

	npm install

根据官网文档能知道:
现在已经支持在项目中运行npm install - -save命令来进行安装react-native的新版本了，例如我们需要更新到0.18版本可以采用终端执行如下的命令:

	npm install --save react-native@0.18

- 更新项目templates文件

新的npm包会包含更新在运行react-native init命令生成的一些动态文件，例如init创建项目的时候会生成iOS和Android的子项目，我们可以通过以下的命令进行获取最新的代码

	react-native upgrade

以上的react-native upgrade会进行检查项目的文件，然后进行如下几个操作:

- 如果是新添加的文件，会进行直接创建
- 如果更新的文件和当前项目的文件是一样的，就会直接忽略跳过
- 如果更新的文件和当前项目的文件不同，有冲突的情况，会让我们进行选择是保留原来的文件还是用更新的文件覆盖，这个要看实际情况了。

现在更新已经完成了，下面就是运行一下看一下是否能够成功运行，运行如下命令:

	react-native run-android

- React Native版本降级方法

刚刚我们已经完成React Native库升级了，现在假如有这样的一个情况，我们的项目直接创建的用了最新版本的，突然发现最新版本可能不太稳定，在开发过程中就会遇到不可预期的bug。那么就可以考虑进行降级到一个比较稳定的版本比较保险。第一种方案我们参考上面的流程就行了，上面是修改成最新版本的，那么现在我们修改一个低版本，然后执行上面的同样的命令就OK了。但是我们降级这边给大家讲第二个方案，还记得上面有一个官方推荐安装react-native的命令不？

	npm install --save react-native@0.18

那么我们现在假如要降级到0.17版本，如下命令行执行一下就OK了。

	npm install --save react-native@0.17

- 总结
无论是升级还是降级只需要两步，先后执行下面两个命令

``` bash
npm install --save react-native@0.18

react-native upgrade
```

## 参考资料： ##

环境安装：
[【React Native开发】React Native For Android环境配置以及第一个实例](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native-for-android%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%E4%BB%A5%E5%8F%8A%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%AE%9E%E4%BE%8B/)
[史上最详细Windows版本搭建安装React Native环境配置](http://www.lcode.org/%E5%8F%B2%E4%B8%8A%E6%9C%80%E8%AF%A6%E7%BB%86windows%E7%89%88%E6%9C%AC%E6%90%AD%E5%BB%BA%E5%AE%89%E8%A3%85react-native%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/)
[【React Native开发】React Native进行签名打包成Apk](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E8%BF%9B%E8%A1%8C%E7%AD%BE%E5%90%8D%E6%89%93%E5%8C%85%E6%88%90apk/)
[【React Native开发】React Native库版本升级(Upgrading)与降级讲解](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E5%BA%93%E7%89%88%E6%9C%AC%E5%8D%87%E7%BA%A7upgrading%E4%B8%8E%E9%99%8D%E7%BA%A7%E8%AE%B2%E8%A7%A3/)

控件资料：
[【React Native开发】React Native控件之View视图讲解](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bview%E8%A7%86%E5%9B%BE%E8%AE%B2%E8%A7%A3/)
[【React Native开发】React Native控件之Text组件讲解](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btext%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A3/)
[【React Native开发】React Native控件之Image组件讲解与美团首页顶部效果实例(10)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bimage%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A3%E4%B8%8E%E7%BE%8E%E5%9B%A2%E9%A6%96%E9%A1%B5%E9%A1%B6%E9%83%A8%E6%95%88/)
[【React Native开发】React Native控件之TextInput组件讲解与QQ登录界面实现(11)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btextinput%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A3%E4%B8%8Eqq%E7%99%BB%E5%BD%95%E7%95%8C%E9%9D%A2%E5%AE%9E%E7%8E%B011/)
[【React Native开发】React Native控件之ProgressBarAndroid进度条讲解(12)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bprogressbarandroid%E8%BF%9B%E5%BA%A6%E6%9D%A1%E8%AE%B2%E8%A7%A312/)
[【React Native开发】React Native控件之DrawerLayoutAndroid抽屉导航切换组件讲解(13)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bdrawerlayoutandroid%E6%8A%BD%E5%B1%89%E5%AF%BC%E8%88%AA%E5%88%87%E6%8D%A2%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A313/)
[【React Native开发】React Native控件之ScrollView组件讲解(14)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bscrollview%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A314/)
[【React Native开发】React Native控件之ToolbarAndroid工具栏控件讲解以及使用(15)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btoolbarandroid%E5%B7%A5%E5%85%B7%E6%A0%8F%E6%8E%A7%E4%BB%B6%E8%AE%B2%E8%A7%A3%E4%BB%A5%E5%8F%8A%E4%BD%BF%E7%94%A8/)
[【React Native开发】React Native控件之Switch与Picker组件讲解以及使用(16)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bswitch%E4%B8%8Epicker%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A3%E4%BB%A5%E5%8F%8A%E4%BD%BF%E7%94%A816/)
[【React Native开发】React Native控件之ViewPagerAndroid讲解以及美团首页顶部效果实例(17)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bviewpagerandroid%E8%AE%B2%E8%A7%A3%E4%BB%A5%E5%8F%8A%E7%BE%8E%E5%9B%A2%E9%A6%96%E9%A1%B5%E9%A1%B6%E9%83%A8/)
[【React Native开发】React Native控件之Touchable*系列组件详解(18)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btouchable%E7%B3%BB%E5%88%97%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A318/)
[【React Native开发】React Native控件之ListView组件讲解以及详细实例(19)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Blistview%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A3%E4%BB%A5%E5%8F%8A%E8%AF%A6%E7%BB%86%E5%AE%9E%E4%BE%8B19/)
[React Native超棒的LayoutAnimation(布局动画)](http://www.lcode.org/react-native%E8%B6%85%E6%A3%92%E7%9A%84layoutanimation%E5%B8%83%E5%B1%80%E5%8A%A8%E7%94%BB/)
[【React Native开发】React Native控件之PullToRefreshViewAndroid下拉刷新组件讲解(20)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bpulltorefreshviewandroid%E4%B8%8B%E6%8B%89%E5%88%B7%E6%96%B0%E7%BB%84%E4%BB%B6%E8%AE%B2%E8%A7%A320/)
[【React Native开发】React Native控件之RefreshControl组件详解(21)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Brefreshcontrol%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A321/)
[【React Native开发】React Native控件之WebView组件详解以及实例使用(22)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bwebview%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3%E4%BB%A5%E5%8F%8A%E5%AE%9E%E4%BE%8B%E4%BD%BF%E7%94%A822/)
[【React Native开发】React Native控件之组件封装实例(Button按钮)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8B%E7%BB%84%E4%BB%B6%E5%B0%81%E8%A3%85%E5%AE%9E%E4%BE%8Bbutton%E6%8C%89%E9%92%AE/)
[【React Native开发】React Native控件之Navigator组件详解以及实例(23)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bnavigator%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3%E4%BB%A5%E5%8F%8A%E5%AE%9E%E4%BE%8B23/)
[【React Native开发】React Native API模块之ToastAndroid详解及使用(24)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Btoastandroid%E8%AF%A6%E8%A7%A3%E5%8F%8A%E4%BD%BF%E7%94%A824/)
[【React Native开发】React Native API模块之Alert弹出框详解及使用(25)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Balert%E5%BC%B9%E5%87%BA%E6%A1%86%E8%AF%A6%E8%A7%A3%E5%8F%8A%E4%BD%BF%E7%94%A825/)
[【React Native开发】React Native API模块之AppState详解(26)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Bappstate%E8%AF%A6%E8%A7%A326/)
[【React Native开发】React Native 控件之Cilpboard粘贴板使用详解(27)](http://www.lcode.org/react-native-%E6%8E%A7%E4%BB%B6%E4%B9%8Bcilpboard%E7%B2%98%E8%B4%B4%E6%9D%BF%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A327/)
[【React Native开发】React Native API模块Dimensions屏幕宽高详解(30)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97dimensions%E5%B1%8F%E5%B9%95%E5%AE%BD%E9%AB%98%E8%AF%A6%E8%A7%A330/)
[【React Native开发】React Native API模块BackAndroid拦截返回键事件处理详解(31)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97backandroid%E6%8B%A6%E6%88%AA%E8%BF%94%E5%9B%9E%E9%94%AE%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E8%AF%A6%E8%A7%A331/)
[【React Native开发】React Native API模块StyleSheet样式表详解(32)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native-api%E6%A8%A1%E5%9D%97stylesheet%E6%A0%B7%E5%BC%8F%E8%A1%A8%E8%AF%A6%E8%A7%A332/)
[【React Native开发】React Native API模块PixelRatio设备像素密度详解(33)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97pixelratio%E8%AE%BE%E5%A4%87%E5%83%8F%E7%B4%A0%E5%AF%86%E5%BA%A6%E8%AF%A6%E8%A7%A333/)
[【React Native开发】React Native控件之DatePickerAndroid时间日期选择器组件讲解(34)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bdatepickerandroid%E6%97%B6%E9%97%B4%E6%97%A5%E6%9C%9F%E9%80%89%E6%8B%A9%E5%99%A8%E7%BB%84%E4%BB%B6%E8%AE%B2/)
[【React Native开发】React Native控件之StatusBar状态栏详解(35)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bstatusbar%E7%8A%B6%E6%80%81%E6%A0%8F%E8%AF%A6%E8%A7%A335/)
[【React Native开发】React Native API模块之AlertIOS弹框详解-适配iOS开发(36)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Balertios%E5%BC%B9%E6%A1%86%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios%E5%BC%80%E5%8F%9136/)
[【React Native开发】React Native控件之PickerIOS选择器详解-适配iOS开发(37)](http://www.lcode.org/react-native-%E6%8E%A7%E4%BB%B6%E4%B9%8Bpickerios%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios%E5%BC%80%E5%8F%9137/)
[【React Native开发】React Native 控件之SegmentedControlIOS分段组件详解-适配iOS开发(38)](http://www.lcode.org/react-native-%E6%8E%A7%E4%BB%B6%E4%B9%8Bsegmentedcontrolios%E5%88%86%E6%AE%B5%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios%E5%BC%80/)
[【React Native开发】React Native控件之SliderIOS滑块组件详解-适配iOS开发(39)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bsliderios%E6%BB%91%E5%9D%97%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios%E5%BC%80%E5%8F%9139/)
[【React Native开发】React Native控件之TimePickerAndroid时间选择器组件详解及实例(43)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btimepickerandroid%E6%97%B6%E9%97%B4%E9%80%89%E6%8B%A9%E5%99%A8%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3%E5%8F%8A/)
[【React Native开发】React Native API模块之AppStateIOS运行状态详解-适配iOS开发(44)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Bappstateios%E8%BF%90%E8%A1%8C%E7%8A%B6%E6%80%81%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios%E5%BC%80%E5%8F%9144/)
[【React Native开发】React Native API模块之ActionSheetIOS可点击弹框详解-适配iOS开发(45)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Bactionsheetios%E5%8F%AF%E7%82%B9%E5%87%BB%E5%BC%B9%E6%A1%86%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios%E5%BC%80/)
[【React Native开发】React Native API模块之Vibration控制设备震动详解(46)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Bvibration%E6%8E%A7%E5%88%B6%E8%AE%BE%E5%A4%87%E9%9C%87%E5%8A%A8%E8%AF%A6%E8%A7%A346/)
[【React Native开发】React Native特定平台代码说明(47)](http://www.lcode.org/react-native%E7%89%B9%E5%AE%9A%E5%B9%B3%E5%8F%B0%E4%BB%A3%E7%A0%81%E8%AF%B4%E6%98%8E47/)
[【React Native开发】React Native API模块之AppRegistry应用注册入口详解(48)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Bappregistry%E5%BA%94%E7%94%A8%E6%B3%A8%E5%86%8C%E5%85%A5%E5%8F%A3%E8%AF%A6%E8%A7%A348/)
[【React Native开发】React Native基础之Linking Libraries链接库配置-适配iOS开发(49)](http://www.lcode.org/react-native%E5%9F%BA%E7%A1%80%E4%B9%8Blinking-libraries%E9%93%BE%E6%8E%A5%E5%BA%93%E9%85%8D%E7%BD%AE-%E9%80%82%E9%85%8Dios%E5%BC%80%E5%8F%9149/)
[【React Native开发】React Native基础之真机设备运行调试应用-适配iOS开发(50)](http://www.lcode.org/react-native%E5%9F%BA%E7%A1%80%E4%B9%8B%E7%9C%9F%E6%9C%BA%E8%AE%BE%E5%A4%87%E8%BF%90%E8%A1%8C%E8%B0%83%E8%AF%95%E5%BA%94%E7%94%A8-%E9%80%82%E9%85%8Dios/)
[【React Native开发】React Native进阶之原生模块封装基础篇1-适配Android开发(51)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9F%E6%A8%A1%E5%9D%97%E7%BB%84%E4%BB%B6%E5%B0%81%E8%A3%85%E5%9F%BA%E7%A1%80%E7%AF%871-%E9%80%82/)
[【React Native开发】React Native基础之从源代码编译详解-适配Android开发(52)](http://www.lcode.org/react-native%E5%9F%BA%E7%A1%80%E4%B9%8B%E4%BB%8E%E6%BA%90%E4%BB%A3%E7%A0%81%E7%BC%96%E8%AF%91%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dandroid%E5%BC%80/)
[【React Native开发】React Native进阶之原生模块特性篇详解-适配Android开发(53)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9F%E6%A8%A1%E5%9D%97%E7%89%B9%E6%80%A7%E7%AF%87%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dandroid/)
[【React Native开发】React Native进阶之原生UI组件封装详解-适配Android开发(54)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9Fui%E7%BB%84%E4%BB%B6%E5%B0%81%E8%A3%85%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dandroid%E5%BC%80/)
[【React Native开发】React Native模块之Linking详解以及实例-Android/iOS双平台通用(55)](http://www.lcode.org/react-native%E6%A8%A1%E5%9D%97%E4%B9%8Blinking%E8%AF%A6%E8%A7%A3%E4%BB%A5%E5%8F%8A%E5%AE%9E%E4%BE%8B-androidios%E5%8F%8C%E5%B9%B3%E5%8F%B0%E9%80%9A/)
[【React Native开发】React Native 控件之Modal详解-Android/iOS双平台通用(56)](http://www.lcode.org/react-native-%E6%8E%A7%E4%BB%B6%E4%B9%8Bmodal%E8%AF%A6%E8%A7%A3-androidios%E5%8F%8C%E5%B9%B3%E5%8F%B0%E9%80%9A%E7%94%A856/)
[【React Native开发】React Native 移植原生iOS平台项目(57)](http://www.lcode.org/react-native-%E7%A7%BB%E6%A4%8D%E5%8E%9F%E7%94%9Fios%E9%A1%B9%E7%9B%AE57/)
[【React Native开发】React Native 进阶之原生混合与数据通信开发详解-适配Android开发(58)](http://www.lcode.org/react-native-%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9F%E6%B7%B7%E5%90%88%E4%B8%8E%E6%95%B0%E6%8D%AE%E9%80%9A%E4%BF%A1%E5%BC%80%E5%8F%91%E8%AF%A6/)
[【React Native开发】React Native进阶之原生模块封装基础篇详解-适配iOS开发(59)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9F%E6%A8%A1%E5%9D%97%E5%B0%81%E8%A3%85%E5%9F%BA%E7%A1%80%E7%AF%87%E8%AF%A6%E8%A7%A3-%E9%80%82/)
[【React Native开发】React Native进阶之原生模块封装特性篇详解-适配iOS开发(60)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9F%E6%A8%A1%E5%9D%97%E5%B0%81%E8%A3%85%E7%89%B9%E6%80%A7%E7%AF%87%E8%AF%A6%E8%A7%A3-%E9%80%82/)
[【React Native开发】React Native 进阶之原生混合与数据通信开发详解-适配iOS开发(61)](http://www.lcode.org/react-native-%E8%BF%9B%E9%98%B6%E4%B9%8B%E5%8E%9F%E7%94%9F%E6%B7%B7%E5%90%88%E4%B8%8E%E6%95%B0%E6%8D%AE%E9%80%9A%E4%BF%A1%E5%BC%80%E5%8F%91%E8%AF%A6%E8%A7%A3-%E9%80%82%E9%85%8Dios/)
[【React Native开发】React Native API模块之LayoutAnimation布局动画详解-Android/iOS通用(62)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Blayoutanimation%E5%B8%83%E5%B1%80%E5%8A%A8%E7%94%BB%E8%AF%A6%E8%A7%A3-androidios%E9%80%9A%E7%94%A862/)
[【React Native开发】React Native基础之核心组件使用教程介绍-Core Components(63)](http://www.lcode.org/react-native%E5%9F%BA%E7%A1%80%E4%B9%8B%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B%E4%BB%8B%E7%BB%8D-core-components63/)
[【React Native开发】React Native进阶之Animated动画库详解-基础篇(64)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8Banimated%E5%8A%A8%E7%94%BB%E5%BA%93%E8%AF%A6%E8%A7%A3-%E5%9F%BA%E7%A1%80%E7%AF%8764/)
[【React Native开发】React Native进阶之Animated动画库详解-实例篇(65)](http://www.lcode.org/react-native%E8%BF%9B%E9%98%B6%E4%B9%8Banimated%E5%8A%A8%E7%94%BB%E5%BA%93%E8%AF%A6%E8%A7%A3-%E5%AE%9E%E4%BE%8B%E7%AF%8765/)
[【React Native开发】React Native模块之InteractionManager(交互管理器)详解(66)](http://www.lcode.org/react-native%E6%A8%A1%E5%9D%97%E4%B9%8Binteractionmanager%E4%BA%A4%E4%BA%92%E7%AE%A1%E7%90%86%E5%99%A8%E8%AF%A6%E8%A7%A366/)
[【React Native开发】React Native模块之Timers(定时器)详解(67)](http://www.lcode.org/react-native%E6%A8%A1%E5%9D%97%E4%B9%8Btimers%E5%AE%9A%E6%97%B6%E5%99%A8%E8%AF%A6%E8%A7%A367/)
[【React Native开发】React Native模块之Share调用系统分享应用详解(68)](http://www.lcode.org/react-native%E6%A8%A1%E5%9D%97%E4%B9%8Bshare%E8%B0%83%E7%94%A8%E7%B3%BB%E7%BB%9F%E5%88%86%E4%BA%AB%E5%BA%94%E7%94%A8%E8%AF%A6%E8%A7%A368/)
[【React Native开发】React Native模块之PermissionsAndroid权限检测与请求应用详解(69)](http://www.lcode.org/react-native%E6%A8%A1%E5%9D%97%E4%B9%8Bpermissionandroid%E6%9D%83%E9%99%90%E6%A3%80%E6%B5%8B%E4%B8%8E%E8%AF%B7%E6%B1%82%E5%BA%94%E7%94%A8%E8%AF%A6/)
[【React Native开发】React Native 基础之Props(属性)与State(状态)使用讲解(70)](http://www.lcode.org/react-native-%E5%9F%BA%E7%A1%80%E4%B9%8Bprops%E5%B1%9E%E6%80%A7%E4%B8%8Estate%E7%8A%B6%E6%80%81%E4%BD%BF%E7%94%A8%E8%AE%B2%E8%A7%A370/)
[【React Native开发】React Native 基础之Style(样式)讲解(71)](http://www.lcode.org/react-native-%E5%9F%BA%E7%A1%80%E4%B9%8Bstyle%E6%A0%B7%E5%BC%8F%E8%AE%B2%E8%A7%A371/)

[移动端数据库新王者-Realm React Native版本应用详解之抛砖引玉入坑篇(一)](http://www.lcode.org/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E6%95%B0%E6%8D%AE%E5%BA%93%E6%96%B0%E7%8E%8B%E8%80%85-realm-react-native%E7%89%88%E6%9C%AC%E5%BA%94%E7%94%A8%E8%AF%A6%E8%A7%A3%E4%B9%8B%E6%8A%9B%E7%A0%96%E5%BC%95%E7%8E%89/)
[移动端数据库新王者-Realm React Native版本应用详解之略陈固陋爬坡篇(二)](http://www.lcode.org/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E6%95%B0%E6%8D%AE%E5%BA%93%E6%96%B0%E7%8E%8B%E8%80%85-realm-react-native%E7%89%88%E6%9C%AC%E5%BA%94%E7%94%A8%E8%AF%A6%E8%A7%A3%E4%B9%8B%E7%95%A5%E9%99%88%E5%9B%BA%E9%99%8B/)
[移动端数据库新王者-Realm React Native版本应用详解之略陈固陋爬坡篇续1(三)](http://www.lcode.org/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E6%95%B0%E6%8D%AE%E5%BA%93%E6%96%B0%E7%8E%8B%E8%80%85-realm-react-native%E7%89%88%E6%9C%AC%E5%BA%94%E7%94%A8%E8%AF%A6%E8%A7%A3%E4%B9%8B%E5%A2%9E%E5%88%A0%E6%94%B9%E6%9F%A5/)

React Native技术文章：
[[319-React Native上海交流会报告]-React Native跨平台开发之旅PPT内容(附下载链接)](http://www.lcode.org/319-react-native-sh-ppt-jiangqq/)
[React Native开发阶段性总结(2016-5-20)](http://www.lcode.org/react-native-developer-zong/)
[[RN实战-嘎嘎商城]之仿快递时间轴布局实现(订单状态)](http://www.lcode.org/rn%E5%AE%9E%E6%88%98-%E5%98%8E%E5%98%8E%E5%95%86%E5%9F%8E%E4%B9%8B%E4%BB%BF%E5%BF%AB%E9%80%92%E6%97%B6%E9%97%B4%E8%BD%B4%E5%B8%83%E5%B1%80%E5%AE%9E%E7%8E%B0%E8%AE%A2%E5%8D%95%E7%8A%B6%E6%80%81/)
[[RN实战-嘎嘎商城]之轻松实现Tab底部菜单导航栏切换效果-Android/iOS双适配](http://www.lcode.org/rn%E5%AE%9E%E6%88%98-%E5%98%8E%E5%98%8E%E5%95%86%E5%9F%8E%E4%B9%8B%E8%BD%BB%E6%9D%BE%E5%AE%9E%E7%8E%B0tab%E5%BA%95%E9%83%A8%E8%8F%9C%E5%8D%95%E5%AF%BC%E8%88%AA%E6%A0%8F%E5%88%87%E6%8D%A2%E6%95%88/)
[[RN实战-嘎嘎商城]之商家详情界面布局分析与实现](http://www.lcode.org/rn%E5%AE%9E%E6%88%98-%E5%98%8E%E5%98%8E%E5%95%86%E5%9F%8E%E4%B9%8B%E5%95%86%E5%AE%B6%E8%AF%A6%E6%83%85%E7%95%8C%E9%9D%A2%E5%B8%83%E5%B1%80%E5%88%86%E6%9E%90%E4%B8%8E%E5%AE%9E%E7%8E%B0/)
[[RN实战-嘎嘎商城]之记一次项目Redux重构](http://www.lcode.org/rn%E5%AE%9E%E6%88%98-%E5%98%8E%E5%98%8E%E5%95%86%E5%9F%8E%E4%B9%8B%E8%AE%B0%E4%B8%80%E6%AC%A1%E9%A1%B9%E7%9B%AEredux%E9%87%8D%E6%9E%84/)
[React Native实战系列教程之自定义原生UI组件和VideoView视频播放器开发](http://www.lcode.org/react-native%E5%AE%9E%E6%88%98%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B%E4%B9%8B%E8%87%AA%E5%AE%9A%E4%B9%89%E5%8E%9F%E7%94%9Fui%E7%BB%84%E4%BB%B6%E5%92%8Cvideoview%E8%A7%86%E9%A2%91%E6%92%AD%E6%94%BE/)

React Native开源控件：
[[译]React Native开源图表组件(react-native-chart-android)](http://www.lcode.org/react-native-chart-android/)
[[译]React Native开源Material Design组件(react-native-material-design)](http://www.lcode.org/react-native-source-material-design/)
[[译]React Native二维码图片生成组件(react-native-qrcode)](http://www.lcode.org/source-react-native-qrcode/)
[[译]React Native下拉菜单组件(react-native-menu)](http://www.lcode.org/react-native-menu/)
[[译]React Native开源图片裁剪组件(react-native-image-cropping-ios)](http://www.lcode.org/react-native-image-cropping-ios/)
[[译]React Native开源仿照Android平台的Toast组件(react-native-sk-toast)](http://www.lcode.org/react-native-sk-toast/)
[[荐]React Native开源Android平台的TabBar效果组件(react-native-android-tabbar)](http://www.lcode.org/react-native-android-tabbar/)
[[译]React Native开源广告轮播组件(react-native-viewpager)](http://www.lcode.org/react-native-viewpager-race/)
[[译]React Native开源高德地图定位组件(react-native-amap-location)](http://www.lcode.org/react-native-amap-location/)
[[译]React Native开源百度地图组件(react-native-baidumap-kit)](http://www.lcode.org/react-native-baidumap-kit/)
[[译]React Native开源获取设备信息组件(react-native-device-info)](http://www.lcode.org/react-native-device-info/)
[[译]React Native开源网络处理-上传下载组件(react-native-networking)](http://www.lcode.org/react-native-networking/)
[[译]React Native开源bootstrap风格按钮组件(react-native-bootstrap-buttons)](http://www.lcode.org/react-native-bootstrap-buttons/)
[[译]React Native开源时间日期选择器组件(react-native-datetime)](http://www.lcode.org/react-native-datetime/)
[[译]React Native开源封装Google地图组件(react-native-google-maps)](http://www.lcode.org/react-native-google-maps/)
[[译]React Native开源iOS图表组件(react-native-ios-charts)](http://www.lcode.org/react-native-ios-charts/)
[[译]React Native开源图片选择器组件(react-native-android-imagepicker)](http://www.lcode.org/react-native-android-imagepicker/)
[[译]React Native开源VLC多媒体播放器组件(react-native-vlc-player)](http://www.lcode.org/react-native-vlc-player/)
[[译]React Native开源SQLite数据库组件(react-native-sqlite-storage)](http://www.lcode.org/react-native-sqlite-storage/)
[[译]React Native开源图片缩放处理组件(react-native-image-zoom)](http://www.lcode.org/react-native-image-zoom/)
[[译]React Native开源即时聊天XMPP IM组件(react-native-xmpp)](http://www.lcode.org/react-native-xmpp/)
[[译]React Native开源PDF阅读器组件(react-native-pdf)](http://www.lcode.org/react-native-pdf/)
[[译]React Native开源仿QQ微信列表左右滑动删除等功能组件(react-native-swipeout)](http://www.lcode.org/react-native-swipeout/)
[[译]React Native开源播放器Video组件(react-native-video)](http://www.lcode.org/react-native-video/)
[React Native开源封装AES,MD5加密模块(react-native-encryption-library)](http://www.lcode.org/react-native-encryption-library/)
[超详细React Native实现微信好友/朋友圈分享功能-Android/iOS双平台通用](http://www.lcode.org/%E8%B6%85%E8%AF%A6%E7%BB%86react-native%E5%AE%9E%E7%8E%B0%E5%BE%AE%E4%BF%A1%E5%A5%BD%E5%8F%8B%E6%9C%8B%E5%8F%8B%E5%9C%88%E5%88%86%E4%BA%AB%E5%8A%9F%E8%83%BD-androidios%E5%8F%8C%E5%B9%B3%E5%8F%B0/)

React Native开源项目：
[React Native开源项目-仿拉勾网iOS客户端](http://www.lcode.org/react-native-lagou-source/)
[React Native开源项目-清水河畔BBS客户端](http://www.lcode.org/uestc-bbs-react-native/)
[React Native开源项目-仿美团iOS客户端](http://www.lcode.org/react-native-meituan-source/)
[React Native开源项目-知乎日报客户端](http://www.lcode.org/react-native%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE-%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5%E5%AE%A2%E6%88%B7%E7%AB%AF/)
[React Native开源项目-豆瓣搜索客户端(基于豆瓣Open API)](http://www.lcode.org/react-native-dou-source/)
[React Native开源项目-新闻客户端(News)](http://www.lcode.org/react-native-news-source/)
[React Native开源项目-码农客户端(iOS版本)](http://www.lcode.org/react-native-source-manong/)
[React Native开源项目-Hacker新闻客户端(Android和iOS)](http://www.lcode.org/react-native-source-hacker/)
[React Native开源项目-Github客户端(Android、iOS)-来自阿里开发人员](http://www.lcode.org/react-native-source-gitfeed/)
[React Native开源项目-iOS资讯头条APP](http://www.lcode.org/react-native-source-zixunapp/)
[React Native开源项目-iOS新浪微博客户端](http://www.lcode.org/react-native-source-weibo/)
[React Native开源项目-仿拉勾网客户端(兼容Android、iOS平台)](http://www.lcode.org/react-native-source-lagou-duo/)
[React Native开源项目-CNode论坛客户端](http://www.lcode.org/react-native-source-cnode/)
[React Native开源项目-干货集中营客户端(Gank.io)](http://www.lcode.org/react-native-source-gankio/)
[reading](https://github.com/attentiveness/reading)
[React Native开源项目-知识点记忆客户端](http://www.lcode.org/react-native-source-memory/)
[React Native开源项目-BBC新闻客户端](http://www.lcode.org/react-native-source-bbc/)
[React Native开源项目-开源中国Git@OSC客户端](http://www.lcode.org/react-native-source-gitosc/)
[React Native开源项目-仿猫眼电影客户端](http://www.lcode.org/react-native-source-maoyan/)
[学习React Native必看的几个开源项目(第一波)](http://www.lcode.org/study-react-native-opensource-one/)
[学习React Native必看的几个开源项目(第二波)](http://www.lcode.org/study-react-native-opensource-two/)
[带大家一步步开发一个电影数据的App(Movie Fetcher)](http://www.lcode.org/react-native-movie-fetcher/)
[React Native开源项目-公司移动OA办公客户端](http://www.lcode.org/react-native-source-office/)
[React Native开源项目-漫威电影客户端](http://www.lcode.org/react-native-source-marvel/)
[React Native开源项目-图片展示客户端RN-BiZhi](http://www.lcode.org/react-native-source-bizhi/)
[React Native开源项目-仿B站客户端(Android)](http://www.lcode.org/react-native-source-bilibili/)
[React Native开源项目-嘎嘎商城客户端(持续更新中)](http://www.lcode.org/react-native-source-gagamall/)
[React Native开源项目-云翻译客户端](http://www.lcode.org/react-native-source-yunfanyi/)
[React Native开源项目-贷贷助手客户端](http://www.lcode.org/react-native-source-daidai/)
[React Native开源项目-仿宝宝健康客户端](http://www.lcode.org/react-native-source-baobao/)
[React Native开源项目-健康养生客户端](http://www.lcode.org/react-native-source-health/)
[React Native开源项目-小BBS客户端](http://www.lcode.org/eact-native-source-bbs/)

React Native工具插件：
[React Native VSCode IDE超强开发插件介绍(智能,代码提醒,运行调试…)](http://www.lcode.org/vscode-react-native-tools/)
[【React Native开发】React Native API模块之NetInfo(网络信息)使用详解(28)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Bnetinfo%E7%BD%91%E7%BB%9C%E4%BF%A1%E6%81%AF%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A328/)
[【React Native开发】React Native API模块之AsyncStorage(持久化存储)使用详解(29)](http://www.lcode.org/react-native-api%E6%A8%A1%E5%9D%97%E4%B9%8Basyncstorage%E6%8C%81%E4%B9%85%E5%8C%96%E5%AD%98%E5%82%A8%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A329/)
[【React Native开发】React Native控件之TabBarIOS和TabBarIOS.Item组件详解及实例(40)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Btabbarios%E5%92%8Ctabbarios-item%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3%E5%8F%8A%E5%AE%9E%E4%BE%8B40/)
[【React Native开发】React Native控件之ProgressViewIOS进度加载组件详解及实例(41)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bprogressviewios%E8%BF%9B%E5%BA%A6%E5%8A%A0%E8%BD%BD%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3%E5%8F%8A%E5%AE%9E/)
[【React Native开发】React Native控件之ActivityIndicatorIOS进度指示器组件详解及实例(42)](http://www.lcode.org/react-native%E6%8E%A7%E4%BB%B6%E4%B9%8Bactivityindicatorios%E8%BF%9B%E5%BA%A6%E6%8C%87%E7%A4%BA%E5%99%A8%E7%BB%84%E4%BB%B6%E8%AF%A6%E8%A7%A3%E5%8F%8A/)

React Native Running And Debugging
[【React Native开发】React Native应用设备运行(Running)以及调试(Debugging)](http://www.lcode.org/%E3%80%90react-native%E5%BC%80%E5%8F%91%E3%80%91react-native%E5%BA%94%E7%94%A8%E8%AE%BE%E5%A4%87%E8%BF%90%E8%A1%8Crunning%E4%BB%A5%E5%8F%8A%E8%B0%83%E8%AF%95debugging/)

编译其它React Native项目：
[超详细Windows版本编译运行React Native官方实例UIExplorer项目(多图慎入)](http://www.lcode.org/%E8%B6%85%E8%AF%A6%E7%BB%86windows%E7%89%88%E6%9C%AC%E7%BC%96%E8%AF%91%E8%BF%90%E8%A1%8Creact-native%E5%AE%98%E6%96%B9%E5%AE%9E%E4%BE%8Buiexplorer%E9%A1%B9%E7%9B%AE%E5%A4%9A%E5%9B%BE%E6%85%8E/)

疑难汇总：
[React Native疑难点,问题深坑最强总结帖(不断更新中)](http://www.lcode.org/react-native%E7%96%91%E9%9A%BE%E7%82%B9%E9%97%AE%E9%A2%98%E6%B7%B1%E5%9D%91%E6%9C%80%E5%BC%BA%E6%80%BB%E7%BB%93%E5%B8%96%E4%B8%8D%E6%96%AD%E6%9B%B4%E6%96%B0%E4%B8%AD/)

Android Studio项目配置：
[优雅的Android Studio项目配置--常用库和版本管理](http://www.lcode.org/%E4%BC%98%E9%9B%85%E7%9A%84android-studio%E9%A1%B9%E7%9B%AE%E9%85%8D%E7%BD%AE-%E5%B8%B8%E7%94%A8%E5%BA%93%E5%92%8C%E7%89%88%E6%9C%AC%E7%AE%A1%E7%90%86/)

Android控件资料：
[Design支持库TabLayout打造仿网易新闻Tab标签效果](http://www.lcode.org/design%E6%94%AF%E6%8C%81%E5%BA%93tablayout%E6%89%93%E9%80%A0%E4%BB%BF%E7%BD%91%E6%98%93%E6%96%B0%E9%97%BBtab%E6%A0%87%E7%AD%BE%E6%95%88%E6%9E%9C/)
[打造QQ6.X最新版本侧滑界面效果实例](http://www.lcode.org/%E6%89%93%E9%80%A0qq6-x%E6%9C%80%E6%96%B0%E7%89%88%E6%9C%AC%E4%BE%A7%E6%BB%91%E7%95%8C%E9%9D%A2%E6%95%88%E6%9E%9C%E5%AE%9E%E4%BE%8B/)

工具技术：
[干货分享:分析Android应用使用的技术框架和开源库](http://www.lcode.org/%E5%B9%B2%E8%B4%A7%E5%88%86%E4%BA%AB%E5%88%86%E6%9E%90android%E5%BA%94%E7%94%A8%E4%BD%BF%E7%94%A8%E7%9A%84%E6%8A%80%E6%9C%AF%E6%A1%86%E6%9E%B6%E5%92%8C%E5%BC%80%E6%BA%90%E5%BA%93/)
[实时消息:JetBrains正式发布Kotlin 1.0，JVM和Android上更好用的语言](http://www.lcode.org/%E5%AE%9E%E6%97%B6%E6%B6%88%E6%81%AFjetbrains%E6%AD%A3%E5%BC%8F%E5%8F%91%E5%B8%83kotlin-1-0%EF%BC%8Cjvm%E5%92%8Candroid%E4%B8%8A%E6%9B%B4%E5%A5%BD%E7%94%A8%E7%9A%84%E8%AF%AD%E8%A8%80/)




