---
layout: post
title: "Glide加载圆角图片，四角出现黑色区域"
date: 2022-05-10 22:12:33 +0530
category: articles
tags: [problem]
---
Glide(3.7)加载圆角图片，四角出现黑色区域问题。

解决方案：  
使用.transform()手动转换解码，直接copy代码：
```html
Glide.with(context)
        .load(imgFile)
        .dontAnimate()
        .transform(new BitmapTransformation(context) {
            @Override
            protected Bitmap transform(BitmapPool pool, Bitmap toTransform, int outWidth, int outHeight) {
                if (toTransform == null) return null;
                Bitmap result = pool.get(toTransform.getWidth(), toTransform.getHeight(), Bitmap.Config.ARGB_8888);
                if (result == null) {
                    result = Bitmap.createBitmap(toTransform.getWidth(), toTransform.getHeight(), Bitmap.Config.ARGB_8888);
                }
                Canvas canvas = new Canvas(result);
                Paint paint = new Paint();
                paint.setShader(new BitmapShader(toTransform, BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP));
                paint.setAntiAlias(true);
                RectF rectF = new RectF(0f, 0f, toTransform.getWidth(), toTransform.getHeight());
                //设置圆角大小
                canvas.drawRoundRect(rectF, DisplayUtil.dp2px(4), DisplayUtil.dp2px(4), paint);
                return result;
            }
            @Override
            public String getId() {
                return "一个id";
            }
            })
        .override(DisplayUtil.dp2px(50),DisplayUtil.dp2px(50))
        .into(imageView);
```

在`getId()`方法需要返回一个id，这个id可以使用自己项目的包名或者其他自定义字符串。