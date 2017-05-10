title: Android Studio删除工程
date: 2016-03-08 10:52:13
categories: Android Studio
tags: Android Studio
---

本人之前一直在用eclipse开发，在业余时间用Android Studio玩了下，发现了许多不一样，然后自己琢磨琢磨发现了一些小东西，现在慢慢总结之删除Project。

<!--more-->

再一次练习中我想删除工程，发现找来找去就是不知道怎么删除Android studio里的工程项目。右键菜单啊，主菜单啊，什么都找不到名叫Delete或者叫Remove的菜单项。直接按Delete键又不能删除掉整个工程项目。天坑啊！eclipse直接按Delete键就可以删除的，这让我怎么玩啊。

于是去网上找了下，很多文章都在说，Android studio里是叫Remove，所以我就到处找。我用Ctrl + Shift + a查找菜单项，查找出来的Remove菜单项也无法删除工程啊。

纠结了很久，最终是阴差阳错之下，才搞定了。

1. 先删除工程的Module。在Menubar中File-->Project Structure-->Module-->左上角红色的减号删除
2. 再选择单个Module右键点击Delete
3. 再删除Project。
第一张图没有Delete菜单项，第二章图才有。
![](http://7xrcic.com1.z0.glb.clouddn.com/as-delete1.png)

![](http://7xrcic.com1.z0.glb.clouddn.com/as-delete2.png)

而且到最后发现Delete根本不能删除最顶层的更目录，只是变成空文件夹而已。。。。。。。已晕

