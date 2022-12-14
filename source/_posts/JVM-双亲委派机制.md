---
title: JVM-双亲委派机制
tags:
- JVM
- Java
  categories:
- 技术
  date:  2022-08-06 20:26:31
  cover: https://images-1301128659.cos.ap-beijing.myqcloud.com/MacBookPro202208051416768.png

---

# 双亲委派机制

## 介绍

​ Java虚拟机对class文件采用的是按需加载的方式，

​ 也就是说当需要使用该类时才会将它的class文件加载到内存生成的class对象。

​ 而且加载某个类的class文件时，java虚拟机采用的是双亲委派模式。

​ 即把请求交由父类处理，它是一种任务委派模式

## 工作原理

![image-20210617212921462](https://images-1301128659.cos.ap-beijing.myqcloud.com/MacBookPro202208051416768.png)

1. ##### 如果一个类加载器收到了类加载的请求，它并不会自己加载，而是先把请求委托给父类的加载器执行

2. ##### 如果父类加载器还有父类，则进一步向上委托，依次递归，请求到达最顶层的引导类加载器。

3. ##### 如果顶层类的加载器加载成功，则成功返回。如果失败，则子加载器会尝试加载。直到加载成功。

## 代码验证

通过查看最顶层父类ClassLoader的loaderClass方法，我们可以验证双亲委派机制。

```java
protected Class<?> loadClass(String name,boolean resolve)
        throws ClassNotFoundException
        {
synchronized (getClassLoadingLock(name)){
        // 首先检查此类是否被加载过了 
        Class<?> c=findLoadedClass(name);
        if(c==null){
        long t0=System.nanoTime();
        try{
        // 调用父类的加载器方法
        if(parent!=null){
        c=parent.loadClass(name,false);
        }else{
        // 此时是最顶级的启动类加载器
        c=findBootstrapClassOrNull(name);
        }
        }catch(ClassNotFoundException e){
        // 抛出异常说明父类无法加载
        }

        if(c==null){
        //父类无法加载的时候，由子类进行加载。
        // to find the class.
        long t1=System.nanoTime();
        c=findClass(name);
        //记录加载时间已经加载耗时
        sun.misc.PerfCounter.getParentDelegationTime().addTime(t1-t0);
        sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
        sun.misc.PerfCounter.getFindClasses().increment();
        }
        }
        if(resolve){
        resolveClass(c);
        }
        return c;
        }
        }
```

## 双亲委派机制优势

- 避免类的重复加载

  当自己程序中定义了一个和Java.lang包同名的类，此时，由于使用的是双亲委派机制，会由启动类加载器去加载`JAVA_HOME/lib`中的类，而不是加载用户自定义的类。此时，程序可以正常编译，但是自己定义的类无法被加载运行。

- 保护程序安全，防止核心API被随意篡改

