---
layout:     post
title:      "阅读安卓开发文档（2.Activity）test"
subtitle:   ""
date:       2016-05-09 
author:     "tangculijier"
header-img: "img/post-bg-2015.jpg"
tags:
    Android
---

### 阅读安卓开发文档（2.Activity)
[google android developer-Activity](http://developer.android.com/intl/zh-cn/guide/components/activities.html)


**先贴出Activity的生命周期**
![java-javascript](/img/in-post/android/activity_lifecycle.png)

**每个生命周期的特征：** 
 
- **OnCreate()**    
Activity被创建。如果onCreate里面直接finish方法，就不会走其他方法，会直接onDestory()。
- **onResume()**  
Called after onRestoreInstanceState(Bundle), onRestart(), or onPause(), for your activity to start interacting with the user. This is a good place to begin animations, open exclusive-access devices (such as the camera), etc.  
有可能在这三个方法之后调用，这是一个开启动画或者专有设备（比如Camera）的好时机。     
  Keep in mind that onResume is not the best indicator that your activity is visible to the user; a system window such as the keyguard may be in front. Use onWindowFocusChanged(boolean) to know for certain that your activity is visible to the user (for example, to resume a game).  
但是它并不意味着你的Acitivity就对用户可见了，有可能系统的window还在你的前面，比如被锁屏的情况下这个acitivity已经OnResume，但是其实不可见。如果想确定是否这个avitivity对用户可见的，可以用onWindowFocusChanged(boolean)方法来确定。它会在onResume后运行。  
例子：acitvity进入焦点  
MainActivity onCreate  
MainActivity onStart  
MainActivity onWindowFocusChanged  hasFocus=true  
当此activity退出时  
MainActivity onWindowFocusChanged  hasFocus=false  
MainActivity onPause  
MainActivity onStop  
MainActivity onDestroy  
  
- **onPause()**  
Called as part of the activity lifecycle when an activity is going into the background, but has not (yet) been killed.   
When activity B is launched in front of activity A, this callback will be invoked on A. B will not be created until A's onPause() returns, so be sure to not do anything lengthy here.  
当Activity要进入后台时触发此方法，但是还没有被杀。
比如当B在A上面启动后，会触发此方法。除非A的onPause方法执行完，否则B不会create.所以要保证不在这个方法内做过多的事情。  
This callback is mostly used for saving any persistent state the activity is editing, to present a "edit in place" model to the user and making sure nothing is lost if there are not enough resources to start the new activity without first killing this one. This is also a good place to do things like stop animations and other things that consume a noticeable amount of CPU in order to make the switch to the next activity as fast as possible, or to close resources that are exclusive access such as the camera.  
这个方法主要用来保存一些Acitivity正在编辑的状态或者是关闭一些动画等事件让cpu更好运转，以便更迅速的开启下一个Activity（UI总是要先展示给用户看----生命周期设计精髓），或者是关闭一些资源，比如Camera.  
After receiving this call you will usually receive a following call to onStop() (after the next activity has been resumed and displayed), however in some cases there will be a direct call back to onResume() without going through the stopped state.  
在这个方法调用后会接着调用onStop,但是要等到下一个Activity被展示出来。
应该使用 onPause() 向存储设备写入至关重要的持久性数据（例如用户编辑）。不过，应该对 onPause()调用期间必须保留的信息有所选择，因为该方法中的任何阻止过程都会妨碍向下一个 Activity 的转变并拖慢用户体验。

- **onStop()**  
Called when you are no longer visible to the user.当对用户完全不可见的时候会调用此方法。  
Note that this method may never be called, in low memory situations where the system does not have enough memory to keep your activity's process running after its onPause() method is called.  
注意这个方法也许就根本不会被调用，当内存严重不足的时候Activity OnPause()后，已经没有足够内存就来执行OnStop();（如上图，直接onPause走左边） 
从MainAcitivity----跳转--->SecondActivity发生的日志顺序：  
MainAcitivity onCreate()  
MainAcitivity onStart()  
MainAcitivity onResume()  
MainAcitivity onPause()  
　　　　　　　　　　　　　　　  SecondActivity onCreate()  
　　　　　　　　　　　　　　　  SecondActivity onStart()  
　　　　　　　　　　　　　　　  SecondActivity onResume()  
MainAcitivity onStop() 

- **onDestroy()**  
在 Activity 被销毁前调用。这是 Activity 将收到的最后调用。 当 Activity 结束（有人调用 Activity 上的 finish()），或系统为节省空间而暂时销毁该 Activity 实例时，可能会调用它。  
您可以通过 [isFinishing()](http://developer.android.com/intl/zh-cn/reference/android/app/Activity.html#isFinishing())方法区分这两种情形。
 

#### 注意事项

**1.结束 Activity注意事项**    

>您可以通过调用 Activity 的 finish() 方法来结束该 Activity。您还可以通过调用 finishActivity() 结束您之前启动的另一个 Activity。
注：在大多数情况下，您不应使用这些方法显式结束 Activity。 正如下文有关 Activity 生命周期的部分所述，Android 系统会为您管理 Activity 的生命周期，因此您无需完成自己的 Activity。 调用这些方法可能对预期的用户体验产生不良影响，因此只应在您确实不想让用户返回此 Activity 实例时使用。





