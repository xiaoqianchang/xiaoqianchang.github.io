title: Hello Hexo
date: 2016-02-24 18:24:10
categories: Hexo
tags: Hexo
---

Welcome to [Hexo](http://hexo.io/)! This is your very first post. Check [documentation](http://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](http://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Hexo 介绍

Hexo 是一个简单地、轻量地、基于Node的一个静态博客框架。通过Hexo我们可以快速创建自己的博客，仅需要几条命令就可以完成。

发布时，Hexo可以部署在自己的Node服务器上面，也可以部署github上面。对于个人用户来说，部署在github上好处颇多，不仅可以省去服务器的成本，还可以减少各种系统运维的麻烦事(系统管理、备份、网络)。所以，基于github的个人站点，正在开始流行起来….

Hexo的官方网站：http://hexo.io/ ，也是基于Github构建的网站。

<!--more-->

## Hexo安装

系统环境：

- win7 64bit

- node v0.10.5

- npm 1.2.19

Hexo安装，要用全局安装，加-g参数。

``` bash
c:\Users\ChangXiao>npm install -g hexo
```

查看hexo的版本

``` bash
c:\Users\ChangXiao>hexo version
```

安装好后，我们就可以使用Hexo创建项目了。

``` bash
c:\Users\ChangXiao>hexo init nodejs-hexo
```

进入目录`cd nodejs-hexo`，并启动Hexo服务器`hexo server`。

这时端口4000被打开了，我们能过浏览器打开地址，http://localhost:4000/ 。

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](http://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](http://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](http://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](http://hexo.io/docs/deployment.html)

## 参考资料： ##

[Hexo在github上构建免费的Web应用](http://blog.fens.me/hexo-blog-github/)
