---
layout: post
title:  "NDK UnsatisfiedLinkError"
date:   2022-05-11 22:07:45 +0800
description: ""
category: articles
tags: [problem]
---
UnsatisfiedLinkError,couldn't find "*.so"

关键日志错误信息(省略部分敏感信息)
```html
 java.lang.UnsatisfiedLinkError: dalvik.system.PathClassLoader[DexPathList[[zip file "*.base.apk"],nativeLibraryDirectories=[/vendor/lib, /system/lib]]] couldn't find "***.so"
        at java.lang.Runtime.loadLibrary(Runtime.java:366)
        at java.lang.System.loadLibrary(System.java:988)
        at com.* Jni.<clinit>(*.java:8)
        at com.* <init>(*.java:26)
```
错误分析: 应用使用到了.so库方法，但是没有找到so库。

原因：还是老古董工程，AS版本为3.5.3，但项目内仍用以前的2.3.3，gradle版本为4.4。在某一天需要修改时发现无法编译通过，便修改项目使用版本为3.5.3。之后出现
以上问题。(祖传代码)

解决:   
在 build.gradle文件(app)下的 defaultConfig{ }内添加NDK配置：
```groovy
 ndk {
    abiFilters "x86", "armeabi" // ...还有其它的cpu架构，根据自己需要配置
 }
```
若以上处理没有解决可尝试在gradle.properties文件添加：(非亲测)
```groovy
 android.useDeprecatedNdk=true
```
