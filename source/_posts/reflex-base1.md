title: 反射基本用法
date: 2016-03-30 11:31:44
categories: Java
tags: 
  - Java
  - reflex
---

这里介绍Java里反射的一些基本方法使用。

## 创建class的三种方式 ##

### 类型.class ###

``` bash
Class c1 = Person.class;
```

### Class.forName ###

``` bash
Class c2 = Class.forName("com.changxiao.Person");
```

### 对象.getClass ###

``` bash
Person p = new Person("张三", "男", 18);
Class c3 = p.getClass();
```

<!--more-->

## 获取类名 ##

``` bash
Person.class.getSimpleName(); // 获取类名：Person
Person.class.getName(); // 获取完整类名：com.changxiao.Person
```

## 属性操作 ##

获取Class：`Class pc = Class.forName("com.changxiao.Person");`

### 获取所有可见(非private)属性 ###

``` bash
Field[] fields = pc.getFields();
```

### 获取所有已声明属性(已声明表示在自己类中声明的任何属性) ###

``` bash
Field[] allFields = pc.getDeclaredFields();
```

### 获取指定的可见的属性 ###

``` bash
// name 非private
Field f1 = pc.getField("name");
```

注意：如果name是private的，则会出现`java.lang.NoSuchFieldException`异常

### 获取指定的属性 ###

``` bash
Field f2 = pc.getDeclaredField("gender");
```

注意：如果gender为private则必须要设置`f2.setAccessible(true);`
如果没有设置后面的属性会出现`java.lang.IllegalAccessException`异常

## 对方法操作 ##

### 获取所有可见的方法 ###

``` bash
Method[] ms1 = pc.getMethods();
```

### 获取所有已声明的方法 ###

``` bash
Method[] ms2 = pc.getDeclaredMethods();
```

### 获取可见的指定方法, 参数列表必须指定, 被获取方法带参数 ###

``` bash
Method m1 = pc.getMethod("setGender", new Class[] { String.class });
```

### 获取可见的指定方法, 参数列表必须指定, 被获取方法不带参数 ###

``` bash
Method m2 = pc.getMethod("getGender", new Class[] { });
```

### 获取已声明的指定方法 ###

``` bash
pc.getDeclaredMethod(name, parameterTypes)
```

## 对构造方法操作 ##

``` bash
pc.getConstructors();
pc.getConstructor(parameterTypes);
```

## 代码展示上述用法 ##

### 反射用法 ###

``` bash
// 创建class的三种方式
// 1. 类型.class
Class c1 = Person.class;
// 2. Class.forName
Class c2 = Class.forName("com.changxiao.Person");
// 3. 对象.getClass
Person p = new Person("张三", "男", 18);
Class c3 = p.getClass();

// 加载出Class
Class pc = Class.forName("com.changxiao.Person");

// 获取类名
String className = pc.getName();
System.out.println(className);



//-------------------对属性操作 start----------------------------------
// 获取所有可见属性成数组
Field[] fields = pc.getFields();
System.out.println(Arrays.toString(fields));

// 获取所有已声明属性(已声明表示在自己类中声明的任何属性)
Field[] allFields = pc.getDeclaredFields();
System.out.println(Arrays.toString(allFields));

// 获取指定的可见的属性
Field f1 = pc.getField("name");
System.out.println(f1);

// 获取指定的属性
Field f2 = pc.getDeclaredField("gender");
System.out.println(f2);
//-------------------对属性操作 end----------------------------------


//-------------------对方法操作 start----------------------------------
// 获取所有可见的方法
Method[] ms1 = pc.getMethods();
System.out.println(Arrays.toString(ms1));

// 获取所有已声明的方法成数组
Method[] ms2 = pc.getDeclaredMethods();
System.out.println(Arrays.toString(ms2));

// 获取可见的指定方法, 参数列表必须指定, 被获取方法带参数
Method m1 = pc.getMethod("setGender", new Class[] { String.class });
//// 获取可见的指定方法, 参数列表必须指定, 被获取方法不带参数
Method m2 = pc.getMethod("getGender", new Class[] { });
System.out.println(m1);
System.out.println(m2);

// 获取已声明的指定名称方法
// pc.getDeclaredMethod(name, parameterTypes)
//-------------------对方法操作 end----------------------------------


//-------------------对构造方法操作 start----------------------------------
//pc.getConstructors()
//pc.getConstructor(parameterTypes)
//-------------------对构造方法操作 end----------------------------------  
```

### 属性Field的使用 ###

``` bash
Person p = new Person("张全蛋", "男", 18);
Class c = Person.class;
// 获取指定的属性描述
// Field nameField = c.getField("name"); // name为非private

// 设置属性的访问权限
Field nameField = c.getDeclaredField("name");
// nameField.setAccessible(true);

// 获取名字
String shuXingMing = nameField.getName();
System.out.println(shuXingMing);

// 设置属性的值 等同于 p.name = "张三";
//		nameField.set(p, "张三");
//		System.out.println(p.getName());

// 通过反射获取指定对象上的属性值
String nameValue = (String) nameField.get(p);
System.out.println(nameValue);
```

输出：
name
张全蛋

### 构造方法Constructor的使用 ###

``` bash
// 获取类型
Class c = Person.class;
/*
// 获取无参构造方法
Constructor con = c.getConstructor(new Class[]{});
// 根据构造方法创建对象
Person p = (Person) con.newInstance(new Object[] {});	
*/

// 获取带参数的构造方法
// 基本数据类型通过封装类.TYPE来获得类别
Constructor con = c.getConstructor(new Class[]{String.class,String.class,Integer.TYPE});
Person p = (Person) con.newInstance(new Object[] {"张三丰", "男", 800});	
System.out.println(p); // 输出对象
```

输出：
Person [name=张三丰, gender=男, age=800]

> 小知识：Integer.TYPE 输出为int

查看Integer的源码：

``` bash
/**
 * The {@code Class} instance representing the primitive type
 * {@code int}.
 *
 * @since   JDK1.1
 */
public static final Class<Integer>  TYPE = (Class<Integer>) Class.getPrimitiveClass("int");
```

### 方法Method的使用 ###

``` bash
Person p1 = new Person("张全蛋", "男", 18);
// 获取类型
Class c = Person.class;

/*
// 获取指定的方法
Method m = c.getDeclaredMethod("eat", new Class[] {});
// 设置可以被访问
m.setAccessible(true);
// 调用对应方法
m.invoke(p1, new Object[] {});
*/

// 获取带参数的方法
Method m1 = c.getDeclaredMethod("setGender", new Class[] {String.class});
// 调用对应方法传递参数
m1.invoke(p1, new Object[] {"male"});

System.out.println(p1.getGender());
```

输出：
male

## 参考资料 ##

[Java反射机制](http://blog.csdn.net/jackiehff/article/details/8509075)

Enjoy It!

