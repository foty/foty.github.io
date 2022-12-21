---
layout: post
title:  "mergeDebugNativeLibs FAILED"
date:   2022-12-21 21:32:8 +0800
categories: articles 
tags: [skill]
---
2 files found with path xxx.so' from inputs

<br>
先看编译报错信息：

```java
2 files found with path 'lib/arm64-v8a/libc++_shared.so' from inputs:
 - C:\Users\manyc\.gradle\caches\transforms-3\eced9061ef9489eab1b72bfb1c1b4bce\transformed\jetified-mmkv-1.2.7\jni\arm64-v8a\libc++_shared.so
 - C:\Users\manyc\.gradle\caches\transforms-3\05f3892a1b17b6e5b2c0257584c3d346\transformed\jetified-???????-1.8.0\jni\arm64-v8a\libc++_shared.so
If you are using jniLibs and CMake IMPORTED targets, see
https://developer.android.com/r/tools/jniLibs-vs-imported-targets
```
从上面报错信息中可以知道2个地方都有`lib/arm64-v8a/libc++_shared.so`的so库。那么就可以得出这个问题大概就是so库重复了。解决办法就是将其中一个依赖库移除，
或者过滤掉其中的so库。移除依赖库显然不可取的，所以只有尝试过滤的操作了。
<p>
解决方法：使用`packagingOptions`。在主module下(app)的build.gradle文件中添加packagingOptions设置(打包配置项)。

```groovy
android {
// ... 省略其他
    packagingOptions {
        pickFirst 'lib/*/libc++_shared.so'
    }
}
```
pickFirst 意思是只选择第一个匹配文件。注意如果把pickFirst后面的内容替换`jni\arm64-v8a\libc++_shared.so`将不会生效。具体原因未探明。暂且记下通
用格式为:
> lib/*/ + 文件资源

<p>
总结下packagingOptions的用法：

* exclude：过滤掉所设置的文件，不参与打包，不添加到apk中。
* pickFirst：匹配到多个相同文件，只选择第一个。
* doNotStrip：设置文件不被优化压缩。
* merge：匹配到多个同名文件时，合并文件，和pickFirst相反。


另外如果没有起作用，检查是否有获取到所有libs、aar等文件：
```groovy
implementation fileTree(dir: "libs", include: ["*.jar", "*.aar"])
```





