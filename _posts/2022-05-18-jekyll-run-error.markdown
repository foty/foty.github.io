---
layout: post
title:  "运行 Jekyll Server 错误解决记录"
date:   2022-05-18 15:15:36 +0800
categories: articles
tags: [problem]
---
记录使用jekyll实现本地预览，执行jekyll server命令的报错日志，以及解决方案

## 报错记录

### 1、could not find gem 'rake(>12.0)' in locally installed gems
解决方案：执行下面命令
```
bundle install
```


### 2、cannot load such file -- webrick (LoadError)
解决：执行下面命令
```
bundle add webrick
```


解决完上面2个错误后，再次执行`jekyll server`，运行成功。回到项目发现项目多出一个`Gemfile.lock`以及`Gemfile`文件会多一行配置。`Gemfile`新增的配置语句：
**gem "webrick", "~> 1.7"**