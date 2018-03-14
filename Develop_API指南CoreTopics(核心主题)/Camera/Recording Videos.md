# Recording Videos(录制视频)

> https://developer.android.google.cn/training/camera/videobasics.html

This lesson explains how to capture video using existing camera applications.

> 本课讲解如何使用现有的相机应用程序来捕捉视频。

Your application has a job to do, and integrating videos is only a small part of it. You want to take videos with minimal fuss, and not reinvent the camcorder. Happily, most Android-powered devices already have a camera application that records video. In this lesson, you make it do this for you.

> 您的应用程序需要完成一项工作，并且整合视频只是其中的一小部分。而不是重新发明摄像机.令人高兴的是，大多数基于Android的设备都有一个可以记录视频的摄像头应用程序。在本课中，你会为你做这件事.

#### Request the camera feature(请求相机功能)

To advertise that your application depends on having a camera, put a <uses-feature> tag in the manifest file:

> 要宣传您的应用程序依赖于拥有摄像头，请在清单文件中放入\<uses-feature\>标签：

```
<manifest ... >
    <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
    ...
</manifest>
```

If your application uses, but does not require a camera in order to function, set android:required to false. In doing so, Google Play will allow devices without a camera to download your application. It's then your responsibility to check for the availability of the camera at runtime by calling hasSystemFeature(PackageManager.FEATURE_CAMERA). If a camera is not available, you should then disable your camera features.

> 如果您的应用程序使用，但不需要相机才能运行，请将android：required设置为false。这样做，Google Play将允许没有摄像头的设备下载您的应用程序。然后，您有责任通过调用hasSystemFeature（PackageManager.FEATURE_CAMERA）来检查运行时相机的可用性。如果相机不可用，则应该禁用相机功能.

#### Record a video with a camera app(用相机应用程序录制视频)

The Android way of delegating actions to other applications is to invoke an Intent that describes what you want done. This process involves three pieces: The Intent itself, a call to start the external Activity, and some code to handle the video when focus returns to your activity.

> 将操作委托给其他应用程序的Android方法是调用描述你想要完成的Intent。此过程涉及三部分：Intent本身，用于启动外部“活动”的调用，以及焦点返回到活动时用于处理视频的一些代码.

Here's a function that invokes an intent to capture video.

> 这是一个调用捕获视频意图的函数。

```
static final int REQUEST_VIDEO_CAPTURE = 1;

private void dispatchTakeVideoIntent() {
    Intent takeVideoIntent = new Intent(MediaStore.ACTION_VIDEO_CAPTURE);
    if (takeVideoIntent.resolveActivity(getPackageManager()) != null) {
        startActivityForResult(takeVideoIntent, REQUEST_VIDEO_CAPTURE);
    }
}
```

Notice that the startActivityForResult() method is protected by a condition that calls resolveActivity(), which returns the first activity component that can handle the intent. Performing this check is important because if you call startActivityForResult() using an intent that no app can handle, your app will crash. So as long as the result is not null, it's safe to use the intent.

> 请注意，startActivityForResult（）方法受调用resolveActivity（）的条件保护，该条件返回可处理意图的第一个活动组件。执行此检查很重要，因为如果您使用无应用程序可以处理的意图调用startActivityForResult（），则您的应用程序将崩溃。所以只要结果不为空，就可以安全使用意图.

##### View the video

The Android Camera application returns the video in the Intent delivered to onActivityResult() as a Uri pointing to the video location in storage. The following code retrieves this video and displays it in a VideoView.

> Android Camera应用程序将Intent中的视频作为Uri返回到onActivityResult（），并指向存储器中的视频位置.以下代码检索此视频并将其显示在VideoView中。

```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent intent) {
    if (requestCode == REQUEST_VIDEO_CAPTURE && resultCode == RESULT_OK) {
        Uri videoUri = intent.getData();
        mVideoView.setVideoURI(videoUri);
    }
}
```

