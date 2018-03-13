## Introduction to Activities_Activity简介

The Activity class is a crucial component of an Android app, and the way activities are launched and put together is a fundamental part of the platform's application model. Unlike programming paradigms in which apps are launched with a main() method, the Android system initiates code in an Activity instance by invoking specific callback methods that correspond to specific stages of its lifecycle.

> Activity类是Android应用程序的重要组成部分，活动的启动和组合是平台应用程序模型的基本组成部分。与使用main（）方法启动应用程序的编程范例不同，Android系统通过调用与其生命周期的特定阶段相对应的特定回调方法在Activity实例中启动代码。

This document introduces the concept of activities, and then provides some lightweight guidance about how to work with them.

> 本文介绍活动的概念，然后提供一些关于如何使用它们的轻量级指导。

#### The concept of activities  活动的概念

The mobile-app experience differs from its desktop counterpart in that a user's interaction with the app doesn't always begin in the same place. Instead, the user journey often begins non-deterministically. For instance, if you open an email app from your home screen, you might see a list of emails. By contrast, if you are using a social media app that then launches your email app, you might go directly to the email app's screen for composing an email.

> 移动应用程序的体验与桌面应用程序的不同之处在于用户与应用程序的交互并不总是始于同一地点。相反，用户旅程通常以非确定性方式开始。例如，如果您从主屏幕打开电子邮件应用程序，则可能会看到一个电子邮件列表。相反，如果您使用的是社交媒体应用程序，然后启动您的电子邮件应用程序，则可以直接进入电子邮件应用程序的屏幕以撰写电子邮件。

The Activity class is designed to facilitate this paradigm. When one app invokes another, the calling app invokes an activity in the other app, rather than the app as an atomic whole. In this way, the activity serves as the entry point for an app's interaction with the user. You implement an activity as a subclass of the Activity class.

> Activity类旨在促进这种范式。当一个应用程序调用另一个应用程序时，调用应用程序在另一个应用程序中调用一个活动，而不是作为原子整体的应用程序。这样，该活动就成为应用程序与用户交互的入口点。您将活动作为Activity类的子类实现。

An activity provides the window in which the app draws its UI. This window typically fills the screen, but may be smaller than the screen and float on top of other windows. Generally, one activity implements one screen in an app. For instance, one of an app’s activities may implement a Preferences screen, while another activity implements a Select Photo screen.

> 一个活动提供了应用程序绘制UI的窗口。此窗口通常填满屏幕，但可能比屏幕小并浮在其他窗口的顶部。通常，一个活动在应用程序中实现一个屏幕。例如，应用程序的一项活动可能会实现“首选项”屏幕，而另一项活动将实施“选择照片”屏幕。

Most apps contain multiple screens, which means they comprise multiple activities. Typically, one activity in an app is specified as the main activity, which is the first screen to appear when the user launches the app. Each activity can then start another activity in order to perform different actions. For example, the main activity in a simple e-mail app may provide the screen that shows an e-mail inbox. From there, the main activity might launch other activities that provide screens for tasks like writing e-mails and opening individual e-mails.

> 大多数应用包含多个屏幕，这意味着它们包含多个活动。通常，应用程序中的一项活动被指定为主要活动，这是用户启动应用程序时显示的第一个屏幕。然后，每项活动都可以开始另一项活动，以便执行不同的操作。例如，简单电子邮件应用程序中的主要活动可能会提供显示电子邮件收件箱的屏幕。从那里开始，主要活动可能会启动其他活动，为撰写电子邮件和打开单个电子邮件等任务提供屏幕。

Although activities work together to form a cohesive user experience in an app, each activity is only loosely bound to the other activities; there are usually minimal dependencies among the activities in an app. In fact, activities often start up activities belonging to other apps. For example, a browser app might launch the Share activity of a social-media app.

> 尽管活动共同协作以在应用中形成一致的用户体验，但每项活动只与其他活动松散地绑定在一起;应用程序中的活动之间通常只有最小的依赖关系。事实上，活动经常启动属于其他应用程序的活动。例如，浏览器应用程序可能会启动社交媒体应用程序的共享活动。

To use activities in your app, you must register information about them in the app’s manifest, and you must manage activity lifecycles appropriately. The rest of this document introduces these subjects.

> 要在您的应用中使用活动，您必须在应用的清单中注册关于它们的信息，并且您必须适当地管理活动生命周期。本文的其余部分介绍了这些主题

#### Configuring the manifest  (配置清单)

For your app to be able to use activities, you must declare the activities, and certain of their attributes, in the manifest.

> 为了让您的应用程序能够使用活动，您必须在清单中声明活动及其某些属性

##### Declare activities (声明activities)

To declare your activity, open your manifest file and add an \<activity\> element as a child of the \<application\> element. For example:

> 要声明您的活动，请打开您的清单文件并添加一个\<activity\>元素作为\<application\>元素的子元素。例如：

```
<manifest ... >
  <application ... >
      <activity android:name=".ExampleActivity" />
      ...
  </application ... >
  ...
</manifest >
```

The only required attribute for this element is android:name, which specifies the class name of the activity. You can also add attributes that define activity characteristics such as label, icon, or UI theme. For more information about these and other attributes, see the \<activity\> element reference documentation.

> 该元素唯一必需的属性是android：name，它指定活动的类名称。您还可以添加定义活动特征的属性，如标签，图标或UI主题。有关这些属性和其他属性的更多信息，请参阅\<activity\>元素参考文档。

> Note: After you publish your app, you should not change activity names. If you do, you might break some functionality, such as app shortcuts. For more information on changes to avoid after publishing, see Things That Cannot Change.

> 注意：发布应用后，您不应更改活动名称。如果这样做，您可能会破坏某些功能，例如应用快捷方式。有关发布后要避免更改的更多信息，请参阅Things That Cannot Change.

##### Declare intent filters (声明意图过滤器)

Intent filters are a very powerful feature of the Android platform. They provide the ability to launch an activity based not only on an explicit request, but also an implicit one. For example, an explicit request might tell the system to “Start the Send Email activity in the Gmail app". By contrast, an implicit request tells the system to “Start a Send Email screen in any activity that can do the job." When the system UI asks a user which app to use in performing a task, that’s an intent filter at work.

> 意图过滤器是Android平台的一个非常强大的功能。他们提供的能力不仅基于明确的请求而且也是隐式的。例如，明确的请求可能会告诉系统“在Gmail应用程序中启动发送电子邮件”活动。相比之下，一个隐含的请求会告诉系统“在任何可以完成这项工作的活动中启动发送电子邮件屏幕”。当系统UI询问用户执行任务时使用哪个应用程序时，这是工作中的意图过滤器。

You can take advantage of this feature by declaring an \<intent-filter\> attribute in the \<activity\> element. The definition of this element includes an \<action\> element and, optionally, a \<category\> element and/or a \<data\> element. These elements combine to specify the type of intent to which your activity can respond. For example, the following code snippet shows how to configure an activity that sends text data, and receives requests from other activities to do so:

> 您可以通过在\<activity\>元素中声明\<intent-filter\>属性来利用此功能。这个元素的定义包括一个\<action\>元素和一个可选的\<category\>元素和/或一个\<data\>元素。这些元素结合在一起来指定您的活动可以响应的意图类型。例如，以下代码片段显示了如何配置发送文本数据的活动，并接收来自其他活动的请求以执行此操作：

```
<activity android:name=".ExampleActivity" android:icon="@drawable/app_icon">
    <intent-filter>
        <action android:name="android.intent.action.SEND" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:mimeType="text/plain" />
    </intent-filter>
</activity>
```

In this example, the \<action\> element specifies that this activity sends data. Declaring the \<category\> element as DEFAULT enables the activity to receive launch requests. The \<data\> element specifies the type of data that this activity can send. The following code snippet shows how to call the activity described above:

> 在这个例子中，\<action\>元素指定这个活动发送数据。将\<category\>元素声明为DEFAULT可使活动接收启动请求。\<data\>元素指定了这个活动可以发送的数据的类型。以下代码片段显示了如何调用上述活动：

```
// Create the text message with a string
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.setType("text/plain");
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
// Start the activity
startActivity(sendIntent);
```

If you intend for your app to be self-contained and not allow other apps to activate its activities, you don't need any other intent filters. Activities that you don't want to make available to other applications should have no intent filters, and you can start them yourself using explicit intents. For more information about how your activities can respond to intents, see Intents and Intent Filters.

> 如果您希望自己的应用是独立的，并且不允许其他应用激活其活动，则不需要任何其他意图过滤器。您不希望提供给其他应用程序的活动应该没有意图过滤器，您可以使用明确的意图自行启动它们。有关您的活动如何响应意图的更多信息，请参阅意图和意图过滤器。

##### Declare permissions（声明权限）

You can use the manifest's \<activity\> tag to control which apps can start a particular activity. A parent activity cannot launch a child activity unless both activities have the same permissions in their manifest. If you declare a \<uses-permission\> element for a particular activity, the calling activity must have a matching \<uses-permission\> element.

> 您可以使用清单的\<activity\>标签来控制哪些应用可以启动特定的活动。除非两个活动在清单中具有相同的权限，否则父级活动无法启动子活动。 如果为特定活动声明\<uses-permission\>元素，则调用活动必须具有匹配的\<uses-permission\>元素。

For example, if your app wants to use a hypothetical app named SocialApp to share a post on social media, SocialApp itself must define the permission that an app calling it must have:

> 例如，如果您的应用想要使用名为SocialApp的假想应用在社交媒体上共享帖子，SocialApp本身必须定义调用它的应用必须具有的权限：

```
<manifest>
<activity android:name="...."
   android:permission=”com.google.socialapp.permission.SHARE_POST”

/>
```

Then, to be allowed to call SocialApp, your app must match the permission set in SocialApp's manifest:

> 然后，要允许调用SocialApp，您的应用必须与SocialApp清单中设置的权限相匹配：

```
<manifest>
   <uses-permission android:name="com.google.socialapp.permission.SHARE_POST" />
</manifest>
```

For more information on permissions and security in general, see Security and Permissions.

> For more information on permissions and security in general, see Security and Permissions.

#### Managing the activity lifecycle (管理活动生命周期)

Over the course of its lifetime, an activity goes through a number of states. You use a series of callbacks to handle transitions between states. The following sections introduce these callbacks.

> 在整个生命周期中，一个活动会经历许多状态。您使用一系列回调来处理状态之间的转换。以下各节介绍这些回调：

###### onCreate()

You must implement this callback, which fires when the system creates your activity. Your implementation should initialize the essential components of your activity: For example, your app should create views and bind data to lists here. Most importantly, this is where you must call setContentView() to define the layout for the activity's user interface。

> 您必须实现此回调，当系统创建您的活动时会首先触发此回调。你的实现应该初始化你的活动的基本组件：例如，您的应用程序应该在此处创建视图并将数据绑定到列表。最重要的是，您必须调用setContentView（）来定义活动用户界面的布局。

When onCreate() finishes, the next callback is always onStart().

> 当onCreate（）完成时，下一个回调总是onStart（）

###### onStart()

As onCreate() exits, the activity enters the Started state, and the activity becomes visible to the user. This callback contains what amounts to the activity’s final preparations for coming to the foreground and becoming interactive.

> 当onCreate（）退出时，活动进入Started状态，活动对用户可见。这个回调包含了什么等于该活动进入前台并成为互动的最后准备.

###### onResume()

The system invokes this callback just before the activity starts interacting with the user. At this point, the activity is at the top of the activity stack, and captures all user input. Most of an app’s core functionality is implemented in the onResume() method.

> 系统在活动开始与用户交互之前调用此回调。此时，该活动位于活动堆栈的顶部，并捕获所有用户输入。应用程序的大部分核心功能都是在onResume（）方法中实现的.

The onPause() callback always follows onResume().

> onPause（）回调总是紧跟onResume（）

###### onPause()

The system calls onPause() when the activity loses focus and enters a Paused state. This state occurs when, for example, the user taps the Back or Recents button. When the system calls onPause() for your activity, it technically means your activity is still partially visible, but most often is an indication that the user is leaving the activity, and the activity will soon enter the Stopped or Resumed state.

> 当活动失去焦点并进入暂停状态时，系统调用onPause（）。例如，当用户点击“返回”或“最近”按钮时，会发生此状态。当系统为您的活动调用onPause（）时，它在技术上意味着您的活动仍然部分可见，但通常表示用户正在离开活动，活动很快会进入已停止或已恢复状态。

An activity in the Paused state may continue to update the UI if the user is expecting the UI to update. Examples of such an activity include one showing a navigation map screen or a media player playing. Even if such activities lose focus, the user expects their UI to continue updating.

> 如果用户希望UI更新，处于暂停状态的活动可能会继续更新UI。这种活动的例子包括显示导航地图屏幕或媒体播放器播放的活动。即使这样的活动失去了重点，用户希望他们的用户界面继续更新。

You should not use onPause() to save application or user data, make network calls, or execute database transactions. For information about saving data, see Saving and restoring activity state.

> 您不应该使用onPause（）来保存应用程序或用户数据，进行网络调用或执行数据库事务。有关保存数据的信息，请参阅保存和恢复活动状态。

Once onPause() finishes executing, the next callback is either onStop() or onResume(), depending on what happens after the activity enters the Paused state.

> 一旦onPause（）完成执行，下一个回调就是onStop（）或onResume（），这取决于活动进入暂停状态后发生了什么。

###### onStop()

The system calls onStop() when the activity is no longer visible to the user. This may happen because the activity is being destroyed, a new activity is starting, or an existing activity is entering a Resumed state and is covering the stopped activity. In all of these cases, the stopped activity is no longer visible at all.

> 当活动对用户不再可见时，系统调用onStop（）。这可能是因为活动正在被销毁，新的活动正在开始，或者现有活动正在进入恢复状态并且正在覆盖已停止的活动。在所有这些情况下，停止的活动完全不再可见。

The next callback that the system calls is either onRestart(), if the activity is coming back to interact with the user, or by onDestroy() if this activity is completely terminating.

> 系统调用的下一个回调是onRestart（），如果活动回来与用户交互，或者如果此活动完全终止，则通过onDestroy（）进行回调。

###### onRestart()

The system invokes this callback when an activity in the Stopped state is about to restart. onRestart() restores the state of the activity from the time that it was stopped.

> 当处于停止状态的活动即将重新启动时，系统会调用此回调。onRestart（）从它停止的时间恢复活动的状态。

This callback is always followed by onStart().

> 这个回调总是跟着onStart（）

###### onDestroy()

The system invokes this callback before an activity is destroyed.

> 系统在活动销毁之前调用此回调。

This callback is the final one that the activity receives. onDestroy() is usually implemented to ensure that all of an activity’s resources are released when the activity, or the process containing it, is destroyed.

> 此回调是活动收到的最后一个回调。onDestroy（）通常用于确保在活动或包含它的进程被销毁时释放所有活动的资源。

This section provides only an introduction to this topic. For a more detailed treatment of the activity lifecycle and its callbacks, see The Activity Lifecycle.

> 本节仅介绍该主题。有关活动生命周期及其回调的更详细的处理，请参阅活动生命周期。