---
title: Apk安装流程
categories:

- 技术
  date: 2022-08-07 18:51:12
  cover: https://images-1301128659.cos.ap-beijing.myqcloud.com/shaoxiongdu/202208071853226.jpeg
  tags:
- 安卓

---

## Apk安装的四种方式：

1. 系统应用安装：没有安装界面，在开机时自动完成。
2. 网络下载应用安装:  没有安装界面，在应用市场完成。
3. ADB命令安装:  没有安装界面，通过命令直接安装。
4. 外部设备安装:  有安装界面，通过SD卡等外部设备安装，由packageInstaller处理安装逻辑。

## 涉及到的目录：

1.system/app ： 系统自带的应用程序，获得root权限才能删除。
2.data/app ： 用户程序安装目录，安装时会把apk文件复制到此目录下。
3.data/data ： 存放应用程序的数据。
4.data/dalvik-cache ： 将apk中的dex文件安装到该目录下，dex文件即为可执行文件。

## 大致安装过程

1. #### 拷贝apk到指定的目录

- 用户安装的apk首先会拷贝到/data/app下
- 但系统出厂的apk文件会被放到/system分区下

2. #### 加载apk、拷贝文件、创建应用的数据目录：

   为了加快APP的启动速度，apk在安装的时候，会首先将APP的可执行文件（dex）拷贝到/data/dalvik-cache目录下，缓存起来。再在/data/data/目录下创建应用程序的数据目录，用来存放应用的数据库、xml文件、cache、二进制的so动态库等。

3. #### 解析apk的AndroidManifest.xml文件：

   在安装apk的过程中，会解析apk的AndroidManifest.xml文件，将apk的权限、应用包名、apk的安装位置、版本、userID等重要信息保存在/data/system/packages.xml文件中。这些操作都是在PackageManagerService中完成的。

4. #### 显示icon图标

   应用程序经过PMS中的逻辑处理后，发送安装成功的广播,
   Launcher收到广播之后，显示其icon。

