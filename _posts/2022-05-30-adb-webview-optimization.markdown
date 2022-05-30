---
layout: post
title:  "WebView 加载网页问题"
date:   2022-05-30 11:16:36 +0800
categories: articles
tags: [problem]
---
WebView 加载网页使用记录


<br>

#### 加载网页出现白屏现象：
(尝试、不保证一定行哈)添加配置代码：
```text
 webView.getSettings().setJavaScriptEnabled(true);
 webView.getSettings().setAllowContentAccess(true);
 webView.getSettings().setAllowFileAccessFromFileURLs(true);
 webView.getSettings().setAppCacheEnabled(true);
 webView.getSettings().setLoadWithOverviewMode(true);
 webView.getSettings().setUseWideViewPort(true);
 webView.getSettings().setPluginState(WebSettings.PluginState.ON);
 
 webView.getSettings().setDomStorageEnabled(true);
 // 5.0设备以上
 webView.getSettings().setMixedContentMode(WebSettings.MIXED_CONTENT_ALWAYS_ALLOW);
```


<br>

#### 更多其他
[更多看这里](https://blog.csdn.net/t12x3456/article/details/13769731/)