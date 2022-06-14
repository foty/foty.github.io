---
layout: post
title:  "Bitmap、Drawable的转换"
date:   2022-05-30 11:31:26 +0800
categories: articles
tags: [skill]
---
图片资源类型相互转换记录


<br>

#### 资源文件中获取图片bitmap(drawable转换成bitmap方式之一)
```text
Bitmap bm = BitmapFactory.decodeResource(getResources(), [资源id]);
```

#### 资源文件中获取drawable
```text
Drawable drawable = getDrawable([资源id]);
```

####  bitmap转换成drawable
```text
Drawable drawable = new BitmapDrawable(bmp);
```

#### drawable转换成bitmap
```text
Resources res = getResources();
Drawable drawable = res.getDrawable([资源id]);
BitmapDrawable bd = (BitmapDrawable) drawable;
Bitmap bm = bd.getBitmap();
```