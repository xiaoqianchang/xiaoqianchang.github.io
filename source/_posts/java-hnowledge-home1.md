title: Java弹药库
date: 2016-03-15 17:17:51
categories: Java
tags: 
  - Java
---

继上一篇Android弹药库后对Java中的知识积累在此，这里会持续更新。

<!--more-->

好，开工啦！

## 获取系统环境变量的值 ##

``` bash
java.lang.System.getenv(String)
```

## Collections工具类用法 ##

### 互换在指定列表中指定位置的元素 ###

``` bash
Collections.swap(list, 1, 3); // 互换在list中1、3位置的元素
```

### 对已有的List集合数据排序 ###

1. 比较的是字符串类型

``` bash
Collections.sort(contactList, new Comparator<User>() {
		@Override
		public int compare(User lhs, User rhs) {
			return lhs.getUsername().compareTo(rhs.getUsername());
		}
	});
```

2. 比较的是int

``` bash
Collections.sort(conversationList, new Comparator<Integer>() {
		@Override
		public int compare(final Integer con1, final Integer con2) {
			if (con1 == con2) {
				return 0;
			} else if (con2 > con1) {
				return 1;
			} else {
				return -1;
			}
		}
	});
```

## 正则表达式 ##

### 以下符号的含义 ###

`^`：开始符号
`$`：结尾符号

`?`：出现0到1次
`+`：出现1哒n次
`*`：出现0到n次

`.`：任意字符
`\w`：包括字母、数字和下划线及“_”，等价于[a-zA-Z0-9_]
`\W`：\w取反
`\d`：包括数字，等价于[0-9]
`\D`：\d的取反
`{n}`：出现n次

例子：
`^[a-zA-Z0-9_$]{6,20}$`：表示有字母、数字、下划线6到20位组成

