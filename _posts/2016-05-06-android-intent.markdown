---
layout:     post
title:      "阅读安卓开发文档（1.Intent）"
subtitle:   ""
date:       2016-05-06 
author:     "tangculijier"
header-img: "img/post-bg-2015.jpg"
tags:
    Android
---

### 阅读安卓开发文档（1.Intent）
最近阅读开发文档 [google android developer](http://developer.android.com/intl/zh-cn/guide/)，现在已经有中文版的了，很方便，这里记下一些需要注意的笔记。

### [Intent](http://developer.android.com/intl/zh-cn/guide/components/intents-filters.html)  

**1.启动Intent注意事项**

>警告：为了确保应用的安全性，启动 Service 时，请始终使用显式 Intent，且不要为服务声明 Intent 过滤器。使用隐式 Intent 启动服务存在安全隐患，因为您无法确定哪些服务将响应 Intent，且用户无法看到哪些服务已启动。从 Android 5.0（API 级别 21）开始，如果使用隐式 Intent 调用 bindService()，系统会抛出异常。

**2.隐式启动Intent注意事项**

>警告：用户可能没有任何应用处理您发送到 startActivity() 的隐式 Intent。如果出现这种情况，则调用将会失败，且应用会崩溃。要验证 Activity 是否会接收 Intent，请对 Intent 对象调用 resolveActivity()。如果结果为非空，则至少有一个应用能够处理该 Intent，且可以安全调用 startActivity()。如果结果为空，则不应使用该 Intent。如有可能，您应禁用发出该 Intent 的功能。

```java
// Create the text message with a string
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
sendIntent.setType("text/plain");

// Verify that the intent will resolve to an activity
if (sendIntent.resolveActivity(getPackageManager()) != null)
{
    startActivity(sendIntent);
}
```
比如官网给的上面这段代码
如果改成这个

```java
	sendIntent.setType("xxx");//sendIntent.setType("text/plain");
```
并且将下面这句注释掉,就会直接报错退出，因为xxx type并没有人相应

```java
	if (sendIntent.resolveActivity(getPackageManager()) != null)
```

```java
	android.content.ActivityNotFoundException: No Activity found to handle
 Intent { act=android.intent.action.SEND typ=xxx flg=0x1 (has clip) (has extras) 
```



