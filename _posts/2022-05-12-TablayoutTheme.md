---
layout: post
title:  "TabLayout主题配置问题"
date:   2022-05-12 10:05:41 +0800
category: articles
tags: [problem]
---
TabLayout: Error inflating class android.support.design.widget.TabLayout

##### TabLayout

原因: 项目要使用一种比较古老风格(项目原因，而不是要做成这个古老)。所有的弹窗提示等都是这种风格。主题样式代码:
```html
    <style name="ThemeNoTitle" parent="android:Theme">
        //...省略代码//
        
    </style>
```
后来引进TabLayout，在它的activity应用这种主题后,产生以下错误：  
`TabLayout: Error inflating class android.support.design.widget.TabLayout`

在错误日志中其实就已经说明错误原因以及解决方案。总的来说是因为包含TabLayout的那个activity的应用主题使用不
恰当，应该使用Theme.AppCompat主题或者继承自Theme.AppCompat的主题。而我的应用主题是继承至`android:Theme`。
处理方案也很明显：直接使用Theme.AppCompat主题或者继承自Theme.AppCompat的主题，或者不应用自定义主题，使用系统
默认的。   
但是这样就不符合我应用的古朴风格了，所以还是应该要去处理这个问题的。在一番探索下发现了一个解决办法。  
仔细看错误信息，发现这个错误抛出的具体位置是在一个TabLayout类中调用ThemeUtils.checkAppCompatTheme(context)方
法导致的。ThemeUtils的代码:
```java
class ThemeUtils {
    private static final int[] APPCOMPAT_CHECK_ATTRS;
    ThemeUtils() {
    }
    static void checkAppCompatTheme(Context context) {
        TypedArray a = context.obtainStyledAttributes(APPCOMPAT_CHECK_ATTRS);
        boolean failed = !a.hasValue(0);
        if (a != null) {
            a.recycle();
        }
        if (failed) {
            throw new IllegalArgumentException("You need to use a Theme.AppCompat theme (or descendant) with the design library.");
        }
    }
    static {
        APPCOMPAT_CHECK_ATTRS = new int[]{attr.colorPrimary};
    }
}
```
从这段代码中可以发现导致抛出异常原因是在TypedArray中没有找到某个或者某些属性，这些属性来自一个APPCOMPAT_CHECK_ATTRS的数组。
看到``attr.colorPrimary``这个很容易就联想到主题当中的某个属性-colorPrimary。如下系统的style下：
```xml
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
```
自己声明的主题又确实没有定义colorPrimary这条。于是在自己的主题下添加这个属性：
```html
    <style name="ThemeNoTitle" parent="android:Theme">
       <item name="colorPrimary">#ffffff</item>
        //...省略代码//
    </style>
```
运行后TabLayout不再报错，问题解决。