---
layout: post
title:  "build.gradle文件变化更新"
date:   2022-06-14 09:08:53 +0800
categories: articles
tags: [skill]
---
build.gradle等配置文件随AS升级更新的变化

<br>

#### 1、buildSrc -> includeBuild


#### 2、groovy(build.gradle) -> kts(.gradle.kts)
<https://blog.csdn.net/qq_20613731/article/details/119766219>


#### 3、apt -> kapt -> ksp


#### 4、AGP7.0
[了解AGP](https://juejin.cn/post/7067779507838517278)

AGP7.1之后，根项目的`build.gradle`不再包括`buildscript`闭包，迁移到了`settings.gradle`，并且写法有很大的变化