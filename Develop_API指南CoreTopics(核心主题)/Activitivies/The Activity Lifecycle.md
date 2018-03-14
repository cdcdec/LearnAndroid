## The Activity Lifecycle

> //https://developer.android.google.cn/guide/components/activities/activity-lifecycle.html#tba

As a user navigates through, out of, and back to your app, the Activity instances in your app transition through different states in their lifecycle. The Activity class provides a number of callbacks that allow the activity to know that a state has changed: that the system is creating, stopping, or resuming an activity, or destroying the process in which the activity resides.

> 当用户浏览，浏览并返回到您的应用程序时，您应用程序中的Activity实例将通过其生命周期中的不同状态进行转换。Activity类提供了许多回调，让活动知道状态已经改变：系统正在创建，停止或恢复活动，或销毁活动所在的进程。

Within the lifecycle callback methods, you can declare how your activity behaves when the user leaves and re-enters the activity. For example, if you're building a streaming video player, you might pause the video and terminate the network connection when the user switches to another app. When the user returns, you can reconnect to the network and allow the user to resume the video from the same spot. In other words, each callback allows you to perform specific work that's appropriate to a given change of state. Doing the right work at the right time and handling transitions properly make your app more robust and performant. For example, good implementation of the lifecycle callbacks can help ensure that your app avoids:

> 在生命周期回调方法中，您可以声明用户离开并重新进入活动时活动的行为。例如，如果您正在构建流媒体视频播放器，则可能会暂停视频并在用户切换到其他应用时终止网络连接。当用户返回时，您可以重新连接到网络并允许用户从同一地点恢复视频。换句话说，每个回调允许您执行适合于给定状态更改的特定工作。在正确的时间做适当的工作并正确地处理转换，可以使您的应用更加健壮和高效。例如，生命周期回调的良好实现可以帮助确保您的应用程序避免：

* Crashing if the user receives a phone call or switches to another app while using your app.

  > 如果用户在使用应用程序时收到电话或切换到其他应用程序，则会崩溃。

* Consuming valuable system resources when the user is not actively using it.

  > 当用户没有使用它时消耗宝贵的系统资源。

* Losing the user's progress if they leave your app and return to it at a later time.

  > 如果用户离开您的应用程序并在稍后返回，则会丢失用户的进度。

* Crashing or losing the user's progress when the screen rotates between landscape and portrait orientation.

  > 当屏幕在横向和纵向之间旋转时，会损坏或丢失用户的进度。


This document explains the activity lifecycle in detail. The document begins by describing the lifecycle paradigm. Next, it explains each of the callbacks: what happens internally while they execute, and what you should implement during them. It then briefly introduces the relationship between activity state and a process’s vulnerability to being killed by the system. Last, it discusses several topics related to transitions between activity states.

> 本文档详细解释了活动生命周期。该文件从描述生命周期范式开始。接下来，它解释了每个回调：它们在执行时发生了什么，以及它们应该执行什么。然后简要介绍活动状态与进程被系统杀死的脆弱性之间的关系。最后，它讨论了与活动状态之间的转换有关的几个主题。

For information about handling lifecycles, including guidance about best practices, see Handling Lifecycles.

> 有关处理生命周期的信息，包括关于最佳实践的指导，请参阅处理生命周期

#### Activity-lifecycle concepts(活动生命周期概念)

To navigate transitions between stages of the activity lifecycle, the Activity class provides a core set of six callbacks: onCreate(), onStart(), onResume(), onPause(), onStop(), and onDestroy(). The system invokes each of these callbacks as an activity enters a new state.

> 为了在活动生命周期的阶段之间导航转换，Activity类提供了六个回调的核心集合：onCreate（），onStart（），onResume（），onPause（），onStop（）和onDestroy（）。当活动进入新状态时，系统调用每个回调。

Figure 1 presents a visual representation of this paradigm.

> 图1展示了这种范例的视觉表现。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Activitivies\activity_lifecycle.png)
Figure 1. A simplified illustration of the activity lifecycle.

As the user begins to leave the activity, the system calls methods to dismantle the activity. In some cases, this dismantlement is only partial; the activity still resides in memory (such as when the user switches to another app), and can still come back to the foreground. If the user returns to that activity, the activity resumes from where the user left off. The system’s likelihood of killing a given process—along with the activities in it—depends on the state of the activity at the time. Activity state and ejection from memory provides more information on the relationship between state and vulnerability to ejection.

> 当用户开始离开活动时，系统会调用方法来解除活动。在某些情况下，这种拆除只是部分;该活动仍驻留在内存中（例如，当用户切换到另一个应用程序时），并且仍然可以回到前台。如果用户返回到该活动，则该活动从用户离开的地方恢复。系统杀死一个给定过程的可能性 - 以及其中的活动 - 取决于当时活动的状态。活动状态和记忆弹射提供了关于状态与弹射脆弱性之间关系的更多信息。

Depending on the complexity of your activity, you probably don't need to implement all the lifecycle methods. However, it's important that you understand each one and implement those that ensure your app behaves the way users expect.

> 根据您的活动的复杂程度，您可能不需要实施所有生命周期方法。但是，重要的是要了解每个人并实施那些可确保您的应用按照用户期望的方式行事的人。

The next section of this document provides detail on the callbacks that you use to handle transitions between states.

> 本文档的下一部分详细介绍了用于处理状态之间转换的回调。

##### Lifecycle callbacks(生命周期回调)

This section provides conceptual and implementation information about the callback methods used during the activity lifecycle.

> 本节提供有关在活动生命周期中使用的回调方法的概念和实现信息。

###### OnCreate()

You must implement this callback, which fires when the system first creates the activity. On activity creation, the activity enters the Created state. In the onCreate() method, you perform basic application startup logic that should happen only once for the entire life of the activity. For example, your implementation of onCreate() might bind data to lists, initialize background threads, and instantiate some class-scope variables. This method receives the parameter savedInstanceState, which is a Bundle object containing the activity's previously saved state. If the activity has never existed before, the value of the Bundle object is null.

> 您必须实现此回调，当系统首次创建活动时触发回调。在活动创建时，活动进入创建状态。在onCreate（）方法中，您执行基本的应用程序启动逻辑，在整个生命周期中应该只发生一次。例如，您的onCreate（）实现可能会将数据绑定到列表，初始化后台线程以及实例化一些类作用域变量。此方法接收参数savedInstanceState，它是一个包含活动先前保存状态的Bundle对象。如果活动从未存在过，则Bundle对象的值为空。

The following example of the onCreate() method shows fundamental setup for the activity, such as declaring the user interface (defined in an XML layout file), defining member variables, and configuring some of the UI. In this example, the XML layout file is specified by passing file’s resource ID R.layout.main_activity to setContentView().

> 下面的onCreate（）方法示例显示了该活动的基本设置，例如声明用户界面（在XML布局文件中定义），定义成员变量以及配置一些UI。在本例中，通过将文件的资源ID R.layout.main_activity传递给setContentView（）来指定XML布局文件。

```
TextView mTextView;

// some transient state for the activity instance
String mGameState;

@Override
public void onCreate(Bundle savedInstanceState) {
    // call the super class onCreate to complete the creation of activity like
    // the view hierarchy
    super.onCreate(savedInstanceState);

    // recovering the instance state
    if (savedInstanceState != null) {
        mGameState = savedInstanceState.getString(GAME_STATE_KEY);
    }

    // set the user interface layout for this activity
    // the layout file is defined in the project res/layout/main_activity.xml file
    setContentView(R.layout.main_activity);

    // initialize member TextView so we can manipulate it later
    mTextView = (TextView) findViewById(R.id.text_view);
}

// This callback is called only when there is a saved instance that is previously saved by using
// onSaveInstanceState(). We restore some state in onCreate(), while we can optionally restore
// other state here, possibly usable after onStart() has completed.
// The savedInstanceState Bundle is same as the one used in onCreate().
@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
    mTextView.setText(savedInstanceState.getString(TEXT_VIEW_KEY));
}

// invoked when the activity may be temporarily destroyed, save the instance state here
@Override
public void onSaveInstanceState(Bundle outState) {
    outState.putString(GAME_STATE_KEY, mGameState);
    outState.putString(TEXT_VIEW_KEY, mTextView.getText());

    // call superclass to save any view hierarchy
    super.onSaveInstanceState(outState);
}
```

As an alternative to defining the XML file and passing it to setContentView(), you can create new View objects in your activity code and build a view hierarchy by inserting new Views into a ViewGroup. You then use that layout by passing the root ViewGroup to setContentView(). For more information about creating a user interface, see the User Interface documentation.

> 作为定义XML文件并将其传递给setContentView（）的替代方法，您可以在活动代码中创建新的View对象，并通过将新视图插入ViewGroup来构建视图层次结构。然后通过将根ViewGroup传递给setContentView（）来使用该布局。有关创建用户界面的更多信息，请参阅用户界面文档。

Your activity does not reside in the Created state. After the onCreate() method finishes execution, the activity enters the Started state, and the system calls the onStart() and onResume() methods in quick succession. The next section explains the onStart() callback.

> 您的活动不处于创建状态。在onCreate（）方法完成执行后，活动进入Started状态，并且系统快速连续调用onStart（）和onResume（）方法。下一节解释onStart（）回调.

###### onStart()

When the activity enters the Started state, the system invokes this callback. The onStart() call makes the activity visible to the user, as the app prepares for the activity to enter the foreground and become interactive. For example, this method is where the app initializes the code that maintains the UI. It might also register a BroadcastReceiver that monitors changes that are reflected in the UI.

> 当活动进入开始状态时，系统调用此回调。onStart（）调用使用户可以看到活动，因为应用程序准备进入前台并变为交互式活动。例如，此方法是应用程序初始化维护UI的代码的位置。它也可能注册一个监视UI中反映的变化的BroadcastReceiver。

The onStart() method completes very quickly and, as with the Created state, the activity does not stay resident in the Started state. Once this callback finishes, the activity enters the Resumed state, and the system invokes the onResume() method.

> onStart（）方法非常快速地完成，并且与创建状态一样，该活动不会保持驻留在Started状态.一旦这个回调完成，活动进入恢复状态，系统调用onResume（）方法.

###### onResume()

When the activity enters the Resumed state, it comes to the foreground, and then the system invokes the onResume() callback. This is the state in which the app interacts with the user. The app stays in this state until something happens to take focus away from the app. Such an event might be, for instance, receiving a phone call, the user’s navigating to another activity, or the device screen’s turning off.

> 当活动进入恢复状态时，它进入前台，然后系统调用onResume（）回调。这是应用程序与用户交互的状态。该应用停留在这种状态，直到发生某些事情将焦点从应用中移开。例如，这种事件可能是接到电话，用户导航到其他活动或设备屏幕关闭。

When an interruptive event occurs, the activity enters the Paused state, and the system invokes the onPause() callback.

> 当发生中断事件时，活动进入暂停状态，系统调用onPause（）回调。

If the activity returns to the Resumed state from the Paused state, the system once again calls onResume() method. For this reason, you should implement onResume() to initialize components that you release during onPause(). For example, you may initialize the camera as follows:

> 如果活动从暂停状态返回到恢复状态，则系统再次调用onResume（）方法。由于这个原因，你应该实现onResume（）来初始化你在onPause（）期间释放的组件。例如，您可以按如下方式初始化相机：

```
@Override
public void onResume() {
    super.onResume();  // Always call the superclass method first

    // Get the Camera instance as the activity achieves full user focus
    //获取相机实例，因为该活动可实现完全的用户焦点
    if (mCamera == null) {
        initializeCamera(); // Local method to handle camera init
        //处理相机初始化的本地方法
    }
}
```

Be aware that the system calls this method every time your activity comes into the foreground, including when it's created for the first time. As such, you should implement onResume() to initialize components that you release during onPause(), and perform any other initializations that must occur each time the activity enters the Resumed state. For example, you should begin animations and initialize components that the activity only uses when it has user focus.

> 请注意，每当您的活动进入前台时（包括第一次创建活动时），系统都会调用此方法。因此，您应该实现onResume（）来初始化您在onPause（）期间释放的组件，并执行每次活动进入恢复状态时必须发生的任何其他初始化。例如，您应该开始动画并初始化活动仅在具有用户焦点时才使用的组件.

###### onPause()

The system calls this method as the first indication that the user is leaving your activity (though it does not always mean the activity is being destroyed). Use the onPause() method to pause operations such animations and music playback that should not continue while the Activity is in the Paused state, and that you expect to resume shortly. There are several reasons why an activity may enter this state. For example:

> 系统调用此方法作为用户离开活动的第一个指示（尽管并不总意味着活动正在被销毁）.使用onPause（）方法可以暂停操作此类动画和音乐播放，在Activity处于暂停状态时不应继续操作，并且您希望很快恢复。活动可能会进入此状态有几个原因。例如：

* Some event interrupts app execution, as described in the onResume() section. This is the most common case.

  > 正如onResume（）部分所述，某些事件会中断应用程序的执行。这是最常见的情况。

* In Android 7.0 (API level 24) or higher, multiple apps run in multi-window mode. Because only one of the apps (windows) has focus at any time, the system pauses all of the other apps.

  > 在Android 7.0（API等级24）或更高版本中，多个应用程序以多窗口模式运行。因为任何时候只有其中一个应用程序（窗口）有焦点，系统会暂停所有其他应用程序。

* A new, semi-transparent activity (such as a dialog) opens. As long as the activity is still partially visible but not in focus, it remains paused.

  > 一个新的，半透明的活动（如对话框）打开。只要该活动仍然部分可见但没有获取焦点，它仍然处于暂停状态。

You can use the onPause() method to release system resources, such as broadcast receivers, handles to sensors (like GPS), or any resources that may affect battery life while your activity is paused and the user does not need them.

> 您可以使用onPause（）方法释放系统资源，例如广播接收器，传感器的手柄（如GPS），或任何资源，这些资源可能会在您的活动暂停并且用户不需要它们时影响电池寿命。

For example, if your application uses the Camera, the onPause() method is a good place to release it. The following example of onPause() is the counterpart to the onResume() example above, releasing the camera that the onResume() example initialized.

> 例如，如果您的应用程序使用相机，则onPause（）方法是释放它的好地方。下面的onPause（）例子是上面的onResume（）例子的对象，释放了onResume（）例子初始化的摄像头。

```
@Override
public void onPause() {
    super.onPause();  // Always call the superclass method first

    // Release the Camera because we don't need it when paused
    // and other activities might need to use it.
    if (mCamera != null) {
        mCamera.release();
        mCamera = null;
    }
}
```

onPause() execution is very brief, and does not necessarily afford enough time to perform save operations. For this reason, you should not use onPause() to save application or user data, make network calls, or execute database transactions; such work may not complete before the method completes. Instead, you should perform heavy-load shutdown operations during onStop(). For more information about suitable operations to perform during onStop(), see onStop(). For more information about saving data, see Saving and restoring activity state.

> onPause（）执行非常简短，并且不一定需要足够的时间来执行保存操作。因此，您不应使用onPause（）来保存应用程序或用户数据，进行网络调用或执行数据库事务;该方法完成前可能无法完成此类工作。相反，您应该在onStop（）期间执行重负载关闭操作。有关在onStop（）期间执行的合适操作的更多信息，请参阅onStop（）。有关保存数据的更多信息，请参阅保存和恢复活动状态。

Completion of the onPause() method does not mean that the activity leaves the Paused state. Rather, the activity remains in this state until either the activity resumes or becomes completely invisible to the user. If the activity resumes, the system once again invokes the onResume() callback. If the activity returns from the Paused state to the Resumed state, the system keeps the Activity instance resident in memory, recalling that instance when it the system invokes onResume(). In this scenario, you don’t need to re-initialize components that were created during any of the callback methods leading up to the Resumed state. If the activity becomes completely invisible, the system calls onStop(). The next section discusses the onStop() callback.

> onPause（）方法的完成并不意味着该活动会离开暂停状态。而是，活动保持这种状态，直到活动恢复或对用户完全不可见为止。如果活动恢复，系统再次调用onResume（）回调。如果活动从Paused状态返回到Resumed状态，系统会将Activity实例驻留在内存中，并在系统调用onResume（）时调用该实例.在这种情况下，您不需要重新初始化任何导致恢复状态的回调方法中创建的组件。如果活动完全不可见，则系统调用onStop（）。下一节讨论onStop（）回调.

###### onStop()

When your activity is no longer visible to the user, it has entered the Stopped state, and the system invokes the onStop() callback. This may occur, for example, when a newly launched activity covers the entire screen. The system may also call onStop() when the activity has finished running, and is about to be terminated.

> 当您的活动对用户不再可见时，它已进入停止状态，并且系统调用onStop（）回调。例如，当新推出的活动覆盖整个屏幕时，可能会发生这种情况。当活动结束运行并且即将被终止时，系统也可以调用onStop（）。

In the onStop() method, the app should release almost all resources that aren't needed while the user is not using it. For example, if you registered a BroadcastReceiver in onStart() to listen for changes that might affect your UI, you can unregister the broadcast receiver in onStop(), as the user can no longer see the UI. It is also important that you use onStop() to release resources that might leak memory, because it is possible for the system to kill the process hosting your activity without calling the activity's final onDestroy() callback.

> 在onStop（）方法中，应用程序应释放几乎所有在用户不使用时不需要的资源。例如，如果您在onStart（）中注册BroadcastReceiver以监听可能会影响UI的更改，则可以在onStop（）中取消注册广播接收器，因为用户无法再看到UI。使用onStop（）释放可能会泄漏内存的资源也很重要，因为系统可能会在不调用活动的最终onDestroy（）回调的情况下终止托管活动的进程。

You should also use onStop() to perform relatively CPU-intensive shutdown operations. For example, if you can't find a more opportune time to save information to a database, you might do so during onStop(). The following example shows an implementation of onStop() that saves the contents of a draft note to persistent storage:

> 您还应该使用onStop（）执行相对CPU密集型关机操作。例如，如果找不到更适合将信息保存到数据库的时间，则可以在onStop（）期间执行此操作。以下示例显示了onStop（）的实现，它将草稿备注的内容保存到永久存储中：

```
@Override
protected void onStop() {
    // call the superclass method first
    super.onStop();

    // save the note's current draft, because the activity is stopping
    // and we want to be sure the current note progress isn't lost.
    ContentValues values = new ContentValues();
    values.put(NotePad.Notes.COLUMN_NAME_NOTE, getCurrentNoteText());
    values.put(NotePad.Notes.COLUMN_NAME_TITLE, getCurrentNoteTitle());

    // do this update in background on an AsyncQueryHandler or equivalent
    mAsyncQueryHandler.startUpdate (
            mToken,  // int token to correlate calls
            null,    // cookie, not used here
            mUri,    // The URI for the note to update.
            values,  // The map of column names and new values to apply to them.
            null,    // No SELECT criteria are used.
            null     // No WHERE columns are used.
    );
}
```

When your activity enters the Stopped state, the Activity object is kept resident in memory: It maintains all state and member information, but is not attached to the window manager. When the activity resumes, the activity recalls this information. You don’t need to re-initialize components that were created during any of the callback methods leading up to the Resumed state. The system also keeps track of the current state for each View object in the layout, so if the user entered text into an EditText widget, that content is retained so you don't need to save and restore it.

> 当您的活动进入停止状态时，活动对象将保持驻留在内存中：它维护所有状态和成员信息，但未附加到窗口管理器。当活动恢复时，活动将回收此信息。您不需要重新初始化任何导致恢复状态的回调方法期间创建的组件。系统还会跟踪布局中每个View对象的当前状态，因此如果用户将文本输入到EditText小部件中，则会保留该内容，因此您无需保存和恢复它。

> Note: Once your activity is stopped, the system might destroy the process that contains the activity if the system needs to recovery memory. Even if the system destroys the process while the activity is stopped, the system still retains the state of the View objects (such as text in an EditText widget) in a Bundle (a blob of key-value pairs) and restores them if the user navigates back to the activity. For more information about restoring an activity to which a user returns, see Saving and restoring activity state.

> 注意：一旦您的活动停止，如果系统需要恢复内存，系统可能会销毁包含该活动的进程。即使系统在活动停止时破坏了流程，系统仍然保留Bundle（键值对中的一对）中View对象的状态（如EditText小部件中的文本），并在用户导航回到活动时恢复它们.有关恢复用户返回的活动的更多信息，请参阅保存和恢复活动状态。

From the Stopped state, the activity either comes back to interact with the user, or the activity is finished running and goes away. If the activity comes back, the system invokes onRestart(). If the Activity is finished running, the system calls onDestroy(). The next section explains the onDestroy() callback.

> 从停止状态，活动要么回到与用户交互，要么活动完成运行并消失。如果活动回来，系统调用onRestart（）。如果Activity完成运行，系统调用onDestroy（）。下一节解释onDestroy（）回调.

###### onDestroy()

Called before the activity is destroyed. This is the final call that the activity receives. The system either invokes this callback because the activity is finishing due to someone's calling finish(), or because the system is temporarily destroying the process containing the activity to save space. You can distinguish between these two scenarios with the isFinishing() method. The system may also call this method when an orientation change occurs, and then immediately call onCreate() to recreate the process (and the components that it contains) in the new orientation.

> 在活动被破坏之前调用。这是活动收到的最终调用。系统要么调用这个回调函数，因为由于某人的调用finish（），活动正在结束，要么系统暂时销毁包含活动的进程以节省空间。你可以用isFinishing（）方法来区分这两种情况。系统也可能在发生方向更改时调用此方法，然后立即调用onCreate（）以新方向重新创建该过程（及其包含的组件）。

The onDestroy() callback releases all resources that have not yet been released by earlier callbacks such as onStop()。

> onDestroy（）回调会释放先前的回调（如onStop（））尚未释放的所有资源。

#### Activity state and ejection from memory（活动状态和从记忆中弹出）

The system never kills an activity directly. Instead, it kills the process in which the activity runs, destroying not only the activity but everything else running in the process, as well.

> 系统不会直接杀死一个活动。相反，它会杀死活动运行的过程，不仅会摧毁活动，还会摧毁流程中运行的所有其他活动。

The system kills processes when it needs to free up RAM; the likelihood of its killing a given process depends on the state of the process at the time. Process state, in turn, depends on the state of the activity running in the process.

> 系统在需要释放RAM时会终止进程;它杀死一个给定过程的可能性取决于当时进程的状态.进程状态又取决于进程中运行的活动的状态。

A user can also kill a process by using the Application Manager under Settings to kill the corresponding app.

> 用户也可以通过使用“设置”下的“应用程序管理器”来终止相应应用程序来终止进程。

Table 1 shows the correlation among process state, activity state, and likelihood of the system’s killing the process.

> 表1显示了进程状态，活动状态和系统杀死进程的可能性之间的相关性。

| Likelihood of being killed(被杀的可能性) | Process state(进程状态)                      | Activity state          |
| ---------------------------------- | ---------------------------------------- | ----------------------- |
| Least                              | Foreground (having or about to get focus) | Created,Started,Resumed |
| More                               | Background (lost focus)                  | Paused                  |
| Most                               | Background (not visible)/Empty           | Stopped/Destroyed       |

Table 1. Relationship between process lifecycle and activity state

> 表1.进程生命周期与活动状态之间的关系

For more information about processes in general, see Processes and Threads. For more information about how the lifecycle of a process is tied to the states of the activities in it, see the Process Lifecycle section of that page。

> 有关一般进程的更多信息，请参阅进程和线程。有关流程生命周期如何与其中的活动状态关联的更多信息，请参阅该页面的流程生命周期部分.

##### Navigating between activities(在活动之间进行导航)

An app is likely to enter and exit an activity, perhaps many times, during the app’s lifetime. For example, the user may tap the device’s Back button, or the activity may need to launch a different activity. This section covers topics you need to know to implement successful activity transitions. These topics include starting an activity from another activity, saving activity state, and restoring activity state.

> 应用程序可能会在应用程序的整个生命周期中进入或退出一个活动，可能很多次。例如，用户可以点击设备的后退按钮，或者该活动可能需要启动不同的活动。本部分介绍实施成功活动转换时需要了解的主题。这些主题包括从其他活动开始活动，保存活动状态以及恢复活动状态。

###### Starting one activity from another

An activity often needs to start another activity at some point. This need arises, for instance, when an app needs to move from the current screen to a new one.

> 一个活动通常需要在某个时候开始另一项活动。例如，当应用程序需要从当前屏幕移动到新屏幕时，就会出现这种需求。

Depending on whether your activity wants a result back from the new activity it’s about to start, you start the new activity using either the startActivity() or the startActivityForResult() method. In either case, you pass in an Intent object.

> 根据您的活动是否需要从新活动返回的结果，您可以使用startActivity（）或startActivityForResult（）方法启动新活动。无论哪种情况，您都会传入一个Intent对象。

The Intent object specifies either the exact activity you want to start or describes the type of action you want to perform (and the system selects the appropriate activity for you, which can even be from a different application). An Intent object can also carry small amounts of data to be used by the activity that is started. For more information about the Intent class, see Intents and Intent Filters.

> Intent对象指定要启动的确切活动或描述您要执行的操作的类型（并且系统会为您选择适当的活动，甚至可能来自不同的应用程序).Intent对象还可以携带少量数据供启动的活动使用。有关Intent类的更多信息，请参阅Intents和Intent Filters。

###### startActivity()

If the newly started activity does not need to return a result, the current activity can start it by calling the startActivity() method.

> 如果新开始的活动不需要返回结果，则当前活动可以通过调用startActivity（）方法来启动它。

When working within your own application, you often need to simply launch a known activity. For example, the following code snippet shows how to launch an activity called SignInActivity.

> 在您的应用程序中工作时，您经常需要简单地启动已知的活动。例如，以下代码片段显示了如何启动一个名为SignInActivity的活动

```
Intent intent = new Intent(this, SignInActivity.class);
startActivity(intent);
```

Your application might also want to perform some action, such as send an email, text message, or status update, using data from your activity. In this case, your application might not have its own activities to perform such actions, so you can instead leverage the activities provided by other applications on the device, which can perform the actions for you. This is where intents are really valuable: You can create an intent that describes an action you want to perform and the system launches the appropriate activity from another application. If there are multiple activities that can handle the intent, then the user can select which one to use. For example, if you want to allow the user to send an email message, you can create the following intent:

> 您的应用程序可能还希望使用您的活动中的数据执行某些操作，例如发送电子邮件，短信或状态更新.在这种情况下，您的应用程序可能没有自己的活动来执行此类操作，因此您可以利用设备上的其他应用程序提供的活动，它们可以为您执行操作。这就是意图真正有价值的地方：您可以创建一个描述您想要执行的操作的意图，并且系统从另一个应用程序启动相应的活动。如果有多个活动可以处理意图，那么用户可以选择使用哪一个.例如，如果您想允许用户发送电子邮件，则可以创建以下意向：

```
Intent intent = new Intent(Intent.ACTION_SEND);
intent.putExtra(Intent.EXTRA_EMAIL, recipientArray);
startActivity(intent);
```

The EXTRA_EMAIL extra added to the intent is a string array of email addresses to which the email should be sent. When an email application responds to this intent, it reads the string array provided in the extra and places them in the "to" field of the email composition form. In this situation, the email application's activity starts and when the user is done, your activity resumes.

> 添加到意图的EXTRA_EMAIL额外是电子邮件应发送到的电子邮件地址的字符串数组。当电子邮件应用程序响应此意图时，它会读取额外提供的字符串数组，并将它们放在电子邮件撰写表单的“到”字段中.在这种情况下，电子邮件应用程序的活动开始，当用户完成时，活动恢复。

###### startActivityForResult()

Sometimes you want to get a result back from an activity when it ends. For example, you may start an activity that lets the user pick a person in a list of contacts; when it ends, it returns the person that was selected. To do this, you call the startActivityForResult(Intent, int) method, where the integer parameter identifies the call. This identifier is meant to disambiguate between multiple calls to startActivityForResult(Intent, int) from the same activity. It's not global identifier and is not at risk of conflicting with other apps or activities.The result comes back through your onActivityResult(int, int, Intent) method.

> 有时您想在活动结束时从活动中取回结果。例如，您可以开始一项活动，让用户在联系人列表中选择一个人;当它结束时，它返回被选中的人。为此，可以调用startActivityForResult（Intent，int）方法，其中integer参数标识调用。此标识符旨在消除来自同一活动的多个对startActivityForResult（Intent，int）的调用之间的歧义。这不是全局标识符，也不会与其他应用程序或活动发生冲突。结果通过onActivityResult（int，int，Intent）方法返回。

When a child activity exits, it can call setResult(int) to return data to its parent. The child activity must always supply a result code, which can be the standard results RESULT_CANCELED, RESULT_OK, or any custom values starting at RESULT_FIRST_USER. In addition, the child activity can optionally return an Intent object containing any additional data it wants. The parent activity uses the onActivityResult(int, int, Intent) method, along with the integer identifier the parent activity originally supplied, to receive the information。

> 当一个子活动退出时，它可以调用setResult（int）将数据返回给它的父级。子活动必须始终提供结果代码，可以是标准结果RESULT_CANCELED，RESULT_OK或从RESULT_FIRST_USER开始的任何自定义值。另外，子活动可以选择返回一个包含任何想要的附加数据的Intent对象。父活动使用onActivityResult（int，int，Intent）方法以及最初提供的父活动的整数标识符来接收信息。

If a child activity fails for any reason, such as crashing, the parent activity receives a result with the code RESULT_CANCELED.

> 如果某个子活动由于某种原因（例如崩溃）而失败，则父活动会收到一个结果，其结果为RESULT_CANCELED.

```
public class MyActivity extends Activity {
     // ...

     static final int PICK_CONTACT_REQUEST = 0;

     public boolean onKeyDown(int keyCode, KeyEvent event) {
         if (keyCode == KeyEvent.KEYCODE_DPAD_CENTER) {
             // When the user center presses, let them pick a contact.
             startActivityForResult(
                 new Intent(Intent.ACTION_PICK,
                 new Uri("content://contacts")),
                 PICK_CONTACT_REQUEST);
            return true;
         }
         return false;
     }

     protected void onActivityResult(int requestCode, int resultCode,
             Intent data) {
         if (requestCode == PICK_CONTACT_REQUEST) {
             if (resultCode == RESULT_OK) {
                 // A contact was picked.  Here we will just display it
                 // to the user.
                 startActivity(new Intent(Intent.ACTION_VIEW, data));
             }
         }
     }
 }
```

######  Coordinating activities（协调活动）



When one activity starts another, they both experience lifecycle transitions. The first activity stops operating and enters the Paused or Stopped state, while the other activity is created. In case these activities share data saved to disc or elsewhere, it's important to understand that the first activity is not completely stopped before the second one is created. Rather, the process of starting the second one overlaps with the process of stopping the first one.

> 当一项活动开始另一项活动时，它们都会经历生命周期转换。第一个Activity停止操作并进入暂停或停止状态，而另一个Activity被创建。如果这些活动共享保存在光盘或其他地方的数据，重要的是要明白，第一个活动在创建第二个活动之前尚未完全停止。相反，启动第二个过程的进程与停止第一个进程的过程重叠。

The order of lifecycle callbacks is well defined, particularly when the two activities are in the same process (app) and one is starting the other. Here's the order of operations that occur when Activity A starts Activity B:

> 生命周期回调的顺序已被很好地定义，特别是当两个活动处于同一个进程（应用程序）并且一个正在启动另一个时。以下是活动A启动活动B时发生的操作顺序：

1.Activity A's onPause() method executes.

> 活动A的onPause（）方法执行。

2.Activity B's onCreate(), onStart(), and onResume() methods execute in sequence. (Activity B now has user focus.)

> Activity B的onCreate（），onStart（）和onResume（）方法按顺序执行。 （活动B现在有用户焦点。）

3.Then, if Activity A is no longer visible on screen, its onStop() method executes.

> 然后，如果活动A在屏幕上不再可见，则执行其onStop（）方法。

###### Saving and restoring activity state(保存和恢复活动状态)

There are a few scenarios in which your activity is destroyed due to normal app behavior, such as when the user presses the Back button or your activity signals its own destruction by calling the finish() method. The system may also destroy the process containing your activity to recover memory if the activity is in the Stopped state and hasn't been used in a long time, or if the foreground activity requires more resources.

> 由于正常的应用程序行为,有几种情况会导致您的活动被破坏:例如，当用户按下“后退”按钮或者您的活动通过调用finish（）方法表示其自身的销毁信号。如果活动处于停止状态并且很长一段时间未使用活动，或者前台活动需要更多资源，系统还可能销毁包含活动的进程以恢复内存。

When your activity is destroyed because the user presses Back or the activity finishes itself, the system's concept of that Activity instance is gone forever because the behavior indicates the activity is no longer needed. However, if the system destroys the activity due to system constraints (rather than normal app behavior), then although the actual Activity instance is gone, the system remembers that it existed such that if the user navigates back to it, the system creates a new instance of the activity using a set of saved data that describes the state of the activity when it was destroyed. The saved data that the system uses to restore the previous state is called the instance state and is a collection of key-value pairs stored in a Bundle object.

> 当您的活动因用户按下“后退”或活动自行完成而被销毁时，该活动实例的系统概念将永远消失，因为该行为表示活动不再需要。但是，如果系统由于系统限制（而不是正常的应用程序行为）而破坏活动，那么虽然实际的Activity实例消失了，系统会记住它存在以至于如果用户导航回到它，系统会使用一组保存的数据创建活动的新实例，这些数据描述活动在销毁时的状态。系统用于恢复先前状态的已保存数据称为实例状态，是存储在Bundle对象中的键值对的集合。

By default, the system uses the Bundle instance state to save information about each View object in your activity layout (such as the text value entered into an EditText widget). So, if your activity instance is destroyed and recreated, the state of the layout is restored to its previous state with no code required by you. However, your activity might have more state information that you'd like to restore, such as member variables that track the user's progress in the activity.

> 默认情况下，系统使用Bundle实例状态来保存有关活动布局中每个View对象的信息（例如输入到EditText小部件中的文本值）。因此，如果您的活动实例被销毁并重新创建，那么布局的状态将恢复到之前的状态，并且不需要您的代码。但是，您的活动可能包含更多想要恢复的状态信息，例如跟踪用户活动进度的成员变量。

A Bundle object isn't appropriate for preserving more than a trivial amount of data because it consumes system-process memory. To preserve more than a very small amount of data, you should take a combined approach to preserving data, using persistent local storage, the onSaveInstanceState() method, and the ViewModel class. For more information about preserving complex data structures, see Saving UI States.

> 一个Bundle对象不适合保存多余的数据量，因为它消耗了系统进程内存。要保留超过极少量的数据，您应该采用组合方法来保存数据，使用持久本地存储，onSaveInstanceState（）方法和ViewModel类。有关保留复杂数据结构的更多信息，请参阅保存UI状态。

For cases where a Bundle is appropriate, you may use the onSaveInstanceState() method. The next section provides detail about how to use this method.

> 对于Bundle合适的情况，您可以使用onSaveInstanceState（）方法。下一节将详细介绍如何使用此方法。

###### Save your activity state(保存你的活动状态)

As your activity begins to stop, the system calls the onSaveInstanceState() method so your activity can save state information with a collection of key-value pairs. The default implementation of this method saves transient information about the state of the activity's view hierarchy, such as the text in an EditText widget or the scroll position of a ListView widget. Your app should implement the onSaveInstanceState() callback after the onPause() method, and before onStop(). Do not implement this callback in onPause().

>当您的活动开始停止时，系统将调用onSaveInstanceState（）方法，以便活动可以使用一组键值对来保存状态信息。此方法的默认实现可以保存有关活动视图层次结构状态的瞬态信息，例如EditText小部件中的文本或ListView小部件的滚动位置。你的应用应该在onPause（）方法之后和onStop（）之前实现onSaveInstanceState（）回调.不要在onPause（）中实现此回调。

> Caution: You must always call the superclass implementation of onSaveInstanceState() so the default implementation can save the state of the view hierarchy.

> 警告：您必须始终调用onSaveInstanceState（）的超类实现，以便默认实现可以保存视图层次结构的状态。

To save additional state information for your activity, you must override onSaveInstanceState() and add key-value pairs to the Bundle object that is saved in the event that your activity is destroyed unexpectedly. For example:

> 为了保存活动的附加状态信息，必须重写onSaveInstanceState（）并将键值对添加到在您的活动意外销毁的事件中保存的Bundle对象。例如:

```
static final String STATE_SCORE = "playerScore";
static final String STATE_LEVEL = "playerLevel";
// ...
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    // Save the user's current game state
    savedInstanceState.putInt(STATE_SCORE, mCurrentScore);
    savedInstanceState.putInt(STATE_LEVEL, mCurrentLevel);


    // Always call the superclass so it can save the view hierarchy state
    super.onSaveInstanceState(savedInstanceState);
}
```

> Note: In order for the Android system to restore the state of the views in your activity, each view must have a unique ID, supplied by the android:id attribute.

> 注意：为了使Android系统恢复活动中视图的状态，每个视图必须具有由android：id属性提供的唯一ID。

To save persistent data, such as user preferences or data for a database, you should take appropriate opportunities when your activity is in the foreground. If no such opportunity arises, you should save such data during the onStop() method.

> 要保存持久性数据（例如用户首选项或数据库数据），当您的活动位于前台时，您应该采取适当的机会。如果没有这样的机会出现，您应该在onStop（）方法中保存这些数据。

###### Restore your activity state(恢复您的活动状态)

When your activity is recreated after it was previously destroyed, you can recover your saved state from the Bundle that the system passes to your activity. Both the onCreate() and onRestoreInstanceState() callback methods receive the same Bundle that contains the instance state information.

> 当您的活动在之前被销毁后重新创建时，您可以从系统传递给您的活动的Bundle中恢复已保存的状态。onCreate（）和onRestoreInstanceState（）回调方法都会收到包含实例状态信息的相同Bundle。

Because the onCreate() method is called whether the system is creating a new instance of your activity or recreating a previous one, you must check whether the state Bundle is null before you attempt to read it. If it is null, then the system is creating a new instance of the activity, instead of restoring a previous one that was destroyed.

> 因为无论系统是创建活动的新实例还是重新创建前一个实例，都会调用onCreate（）方法，所以在尝试读取状态之前，必须检查状态Bundle是否为空。如果它为空，那么系统正在创建一个活动的新实例，而不是恢复之前被销毁的实例。

For example, the following code snippet shows how you can restore some state data in onCreate():

> 例如，以下代码片段显示了如何在onCreate（）中恢复某些状态数据：

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState); // Always call the superclass first


    // Check whether we're recreating a previously destroyed instance
    if (savedInstanceState != null) {
        // Restore value of members from saved state
        mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
        mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
    } else {
        // Probably initialize members with default values for a new instance
    }
    // ...
}
```

Instead of restoring the state during onCreate() you may choose to implement onRestoreInstanceState(), which the system calls after the onStart() method. The system calls onRestoreInstanceState() only if there is a saved state to restore, so you do not need to check whether the Bundle is null:

> 代替在onCreate()方法中恢复数据,你可以选择实现onRestoreInstanceState（），当系统回调onstart()方法之后.只有存在要保存的需要恢复的状态时，系统才会调用onRestoreInstanceState（），因此您不需要检查该Bundle是否为空：

```
public void onRestoreInstanceState(Bundle savedInstanceState) {
    // Always call the superclass so it can restore the view hierarchy
    super.onRestoreInstanceState(savedInstanceState);

    // Restore state members from saved instance
    mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
    mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
}
```

> Caution: Always call the superclass implementation of onRestoreInstanceState() so the default implementation can restore the state of the view hierarchy.

> 警告：始终调用onRestoreInstanceState（）的超类实现，以便默认实现可以恢复视图层次结构的状态。