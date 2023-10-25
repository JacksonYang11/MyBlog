---
title: Android手机屏幕方向的几种表示方法
date: 2023-10-25 23:29:25
tags:
    - programming
    - coding
    - android
cover: /images/bg/road.jpg
---

# Configuration方向

用于表示整个设备的屏幕方向，但不是设备的物理方向，它是根据设备的物理尺寸来确定的，而不是根据设备的旋转角度。这个方向是设备级别的，它代表了整个设备的屏幕方向，对所有的 Activity 都是一致的，它会影响整个设备的屏幕方向，因此会影响所有的 Activity。可以通过getResources().getConfiguration().orientation来获取。

- **Configuration.ORIENTATION_UNDEFINED = 0;**   //表示屏幕方向无法确定，可能是由于设备不支持方向检测或者无法获取到方向信息
- **Configuration.ORIENTATION_PORTRAIT = 1;**    //表示竖屏模式，即设备的高度大于宽度
- **Configuration.ORIENTATION_LANDSCAPE = 2;**   //表示横屏模式，即设备的宽度大于高度
- **Configuration.ORIENTATION_SQUARE = 3;**      //表示正方形模式，即设备的宽度和高度相等

# ActivityInfo方向

表示单个 Activity 的显示方向，每个 Activity 都可以有自己的方向设置，彼此之间不会影响。
可以通过设置 Activity 的属性或调用 setRequestedOrientation() 方法来指定 Activity 的方向，可以通过getRequestedOrientation()方法来获取 Activity 的方向。

- **ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED = -1**
- **ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE = 0**
- **ActivityInfo.SCREEN_ORIENTATION_PORTRAIT = 1**
- **ActivityInfo.SCREEN_ORIENTATION_USER = 2**
- **ActivityInfo.SCREEN_ORIENTATION_BEHIND = 3**
- **ActivityInfo.SCREEN_ORIENTATION_SENSOR = 4**
- **ActivityInfo.SCREEN_ORIENTATION_REVERSE_LANDSCAPE = 8**

# Surface方向

表示设备的物理旋转角度，即设备的屏幕相对于地面的方向。
可以通过((WindowManager) mActivity.getSystemService(Context.WINDOW_SERVICE)).getDefaultDisplay().getRotation()或activity.getWindowManager().getDefaultDisplay().getRotation()来获取。

- **Surface.ROTATION_0 = 0，表示设备的自然方向（横屏或竖屏）**
- **Surface.ROTATION_90 = 1，表示设备顺时针旋转90度的方向**
- **Surface.ROTATION_180 = 2，表示设备顺时针旋转180度的方向**
- **Surface.ROTATION_270 = 3，表示设备顺时针旋转270度的方向**

WindowManager: rotation changed from 1 to 3 表示屏幕的旋转方向从1变为3，指的就是这里的旋转角度变化从顺时针旋转90度变为顺时针旋转270度。

# 各自适用的场景和区别

Configuration 方向、ActivityInfo 方向和 Surface 方向是用于不同的场景，具有以下区别：
1. Configuration 方向：
- Configuration 中的方向用于表示整个设备的屏幕方向。
- 它提供了设备当前的屏幕方向信息，可以通过 getResources().getConfiguration().orientation 来获取。
- 适用于需要根据设备的屏幕方向来调整应用程序布局、资源等的情况。
- 可以在 res 目录下的不同配置文件夹中提供不同方向的布局资源，以适配不同的屏幕方向。
2. ActivityInfo 方向：
- ActivityInfo 中的方向用于指定单个 Activity 的显示方向。
- 通过设置 Activity 的属性或调用 setRequestedOrientation() 方法来指定 Activity 的方向。
- 适用于需要控制单个 Activity 的显示方向的情况，可以实现特定 Activity 的方向控制，而不受设备的屏幕方向影响。
- 可以根据需求设置横屏、竖屏或其他方向，例如在游戏中需要固定为横屏模式。
3. Surface 方向：
- Surface 中的方向用于表示绘制表面（Surface）的方向，通常与设备的屏幕方向相关联。
- 在使用 SurfaceView 或 TextureView 进行图形渲染时，可以通过 setRotation() 方法设置 Surface 的旋转角度来调整绘制表面的方向。
- 适用于需要在绘制表面上进行图形渲染的情况，可以根据设备的屏幕方向来调整图形的显示方向。
  需要注意的是，这三种方向在某些情况下可能会相互影响。例如，当设备的屏幕方向发生变化时，系统会自动调整 Activity 的方向，从而可能导致 Surface 的方向也发生变化。在这种情况下，可以通过监听设备方向变化的事件，并相应地调整 Surface 或其他相关组件的方向来保持一致性。
  总结来说：
- Configuration 方向适用于根据设备的屏幕方向来调整应用程序的布局和资源。
- ActivityInfo 方向适用于控制单个 Activity 的显示方向，可以实现特定 Activity 的方向控制。
- Surface 方向适用于调整绘制表面的方向，通常与设备的屏幕方向相关联，用于图形渲染等场景。

# 使用注意点

ActivityInfo 中的方向设置可以覆盖 Configuration 中的方向设置。如果在 Activity 的属性或方法中指定了特定的方向，那么该 Activity 将按照指定的方向进行显示，而不考虑设备的屏幕方向。这样可以实现单个 Activity 的方向控制，而不受设备的屏幕方向影响。