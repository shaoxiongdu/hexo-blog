---
title: JVM-类加载器的常用方法
tags:
- JVM
- Java
  categories:
- 技术
  date:  2022-08-06 20:26:31
  cover: https://images-1301128659.cos.ap-beijing.myqcloud.com/MacBookPro202208051415566.png

---

# 类加载的常用方法

## 常用方法

#### ClassLoader类，它是一个抽象类，其后所有的类加载器都继承自ClassLoader（不包括启动类加载器）

| 方法名称                                             | 描述                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| getParent（）                                        | 返回该类加载器的超类加载器                                   |
| loadClass（String name）                             | 加载名称为name的类，返回结果为java.lang.Class类的实例        |
| findClass（String name）                             | 查找名称为name的类，返回结果为java.lang.Class类的实例        |
| findLoadedClass（String name）                       | 查找名称为name的已经被加载过的类，返回结果为java.lang.Class类的实例 |
| defineClass（String name，byte[] b,int off,int len） | 把字节数组b中的内容转换为一个Java类 ，返回结果为java.lang.Class类的实例 |
| resolveClass（Class<?> c）                           | 连接指定的一个java类                                         |

## ClassLoader继承关系

![image-20210617214555582](https://images-1301128659.cos.ap-beijing.myqcloud.com/MacBookPro202208051415566.png)

## 获取ClassLoader的途径

![image-20210617214611472](https://images-1301128659.cos.ap-beijing.myqcloud.com/MacBookPro202208051415647.png)
