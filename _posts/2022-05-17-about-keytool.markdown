---
layout: post
title: "Android秘钥工具Keytool使用命令"
date: 2022-05-17 18:01:22 +0890
category: articles
tags: [skill]
---
正常使用keytool命令需要在keytool工具所在的文件目录下执行。也就是studio安装目录下的`jre\bin`目录下



### 查看某个.jks文件
```
keytool -list -v -keystore [1] -storepass [2]
```
[1] 输入查看的jks文件名。如：jk.jks  
[2] 输入ks的密码。


### 删除jks文件
```
keytool -delete -alias [1] -keystore [2]
```
[1]：输入删除jks文件的别名，也就是`alias`属性；  
[2]：输入删除jks文件的名称。如：`jk.jks`

### 创建jks文件
```
keytool -genkey -v -keystore [1] -alias [2]  -storepass [3] -keypass [4] -keyalg RSA -validity [5]
```
* [1] 输入jks文件名称。默认情况下创建的jks文件在**keytool.exe**同目录下。也就是studio安装目录下的` \jre\bin\ `
* [2] 输入别名`alias`
* [3] 输入ks的密码
* [4] 输入key的密码
* [5] 输入有效期,单位为[天]

### 转换PKCS12
```
keytool -importkeystore -srckeystore [1] -destkeystore [2] -deststoretype pkcs12
```
[1]：输入需要转换的jks文件名称。如：`jk.jks`  
[2]：输入转换后的jks文件名称。如：`jk.jks`。可以与[1]文件名称相同，会自动替换。

```
keytool -list -v -keystore [1] -storepass [2]
```
[1] 输入查看的jks文件名。如：jk.jks  
[2] 输入ks的密码。


### 删除jks文件
```
keytool -delete -alias [1] -keystore [2]
```
[1]：输入删除jks文件的别名，也就是`alias`属性；  
[2]：输入删除jks文件的名称。如：`jk.jks`

### 创建jks文件
```
keytool -genkey -v -keystore [1] -alias [2]  -storepass [3] -keypass [4] -keyalg RSA -validity [5]
```
* [1] 输入jks文件名称。默认情况下创建的jks文件在**keytool.exe**同目录下。也就是studio安装目录下的` \jre\bin\ `
* [2] 输入别名`alias`
* [3] 输入ks的密码
* [4] 输入key的密码
* [5] 输入有效期,单位为[天]

### 转换PKCS12
```
keytool -importkeystore -srckeystore [1] -destkeystore [2] -deststoretype pkcs12
```
[1]：输入需要转换的jks文件名称。如：`jk.jks`  
[2]：输入转换后的jks文件名称。如：`jk.jks`。可以与[1]文件名称相同，会自动替换。