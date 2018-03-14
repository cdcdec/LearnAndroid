# Controlling the Camera﻿

> https://developer.android.google.cn/training/camera/cameradirect.html

In this lesson, we discuss how to control the camera hardware directly using the framework APIs.

> 在本课中，我们将讨论如何使用框架API直接控制相机硬件。

Directly controlling a device camera requires a lot more code than requesting pictures or videos from existing camera applications. However, if you want to build a specialized camera application or something fully integrated in your app UI, this lesson shows you how.

> 直接控制设备摄像头比从现有摄像头应用程序请求图片或视频需要更多的代码。但是，如果您想要构建专门的相机应用程序或完全集成在应用程序UI中的内容，本课将告诉您如何实现.

#### Open the Camera Object(打开相机对象)

Getting an instance of the Camera object is the first step in the process of directly controlling the camera. As Android's own Camera application does, the recommended way to access the camera is to open Camera on a separate thread that's launched from onCreate(). This approach is a good idea since it can take a while and might bog down the UI thread. In a more basic implementation, opening the camera can be deferred to the onResume() method to facilitate code reuse and keep the flow of control simple.

>