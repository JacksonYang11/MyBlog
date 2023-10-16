---
title: ViewStub的介绍和使用
date: 2023-10-16 16:31:26
tags:
    - programming
    - coding
    - software development 
---


# 什么是ViewStub?
ViewStub 是一个不可见的，大小为0的视图，可以在运行过程中延时加载布局资源，相当于为可能添加也可能不添加的View占个位。
例：
```
 <ViewStub android:id="@+id/stub" 
     android:inflatedId="@+id/subTree" 
     android:layout="@layout/mySubTree" 
     android:layout_width="120dip" 
     android:layout_height="40dip" />
```

# 为什么要用ViewStub？
当我们需要根据某个条件控制某个View的显示或者隐藏的时候，通常是把可能用到的View都写在布局上，然后设置可见性为View.GONE或View.InVisible ，之后在代码中根据条件动态控制可见性。虽然操作简单，但是耗费资源，因为即便该view不可见，仍会被父窗体绘制，仍会创建对象，仍会被实例化，仍会被设置属性。
而android.view.ViewStub，是一个大小为0 ，默认不可见的控件，只有给他设置成了View.Visible或调用了它的inflate()之后才会填充布局资源，也就是说占用资源少。所以，推荐使用viewStub。

# ViewStub的替换怎么做到？
调用ViewStub的inflate()方法或设置其可见性可以实现替换。替换后原来的ViewStub在视图树中就不存在了，而是替换成它的android:layout属性中的布局，这个布局可以用android:inflatedId属性值获取，也可以通过它自己的android:id获取，新布局的布局参数跟随ViewStub的布局参数。
操作如下：
&nbsp;a.设置可见性：
```java
 viewStub = (ViewStub) findViewById(R.id.viewstub_te（注意：st); 
 viewStub.setVisibility(View.INVISIBLE);
```
&nbsp;b.调用inflate方法：
```java
ViewStub stub = (ViewStub) findViewById(R.id.stub); 
View inflated = stub.inflate();
```
（注意：inflate() 方法只能被调用一次，如果再次调用会报异常信息 ViewStub must have a non-null ViewGroup viewParent。
ViewStub的setVisibility()可以调用多次）

# 参考链接：<https://www.jianshu.com/p/175096cd89ac>

