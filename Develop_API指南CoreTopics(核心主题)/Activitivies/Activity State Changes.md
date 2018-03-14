# Activity State Changes

Different events, some user-triggered and some system-triggered, can cause an Activity to transition from one state to another. This document describes some common cases in which such transitions happen, and how to handle those transitions.

> 不同的事件，一些用户触发和一些系统触发，可能导致活动从一种状态转换到另一种状态。﻿本文档描述了发生此类转换的一些常见情况以及如何处理这些转换。

For more information about activity states, see The Activity Lifecycle.

> 有关活动状态的更多信息，请参阅活动生命周期。

#### Configuration change occurs(发生配置更改)

There are a number of events that can trigger a configuration change. Perhaps the most prominent example is a change between portrait and landscape orientations. Other cases that can cause configuration changes include changes to language or input device.

> 有许多事件可以触发配置更改。也许最突出的例子是纵向和横向取向之间的变化。其他可能导致配置更改的情况包括对语言或输入设备的更改。

When a configuration change occurs, the activity is destroyed and recreated. To preserve simple transient-state data for the activity, you must override the onSaveInstanceState() method to save the data, and then use onCreate() or onRestoreInstanceState() callbacks to recreate the instance state.

> 当发生配置更改时，该活动将被销毁并重新创建。要为活动保留简单的瞬态数据，您必须重写onSaveInstanceState（）方法以保存数据，然后使用onCreate（）或onRestoreInstanceState（）回调重新创建实例状态。

For specific detail about persisting simple data across configuration changes, see the Saving and restoring activity state section of The Activity Lifecycle. For more information about approaches to persisting both simple and complex UI data, see Saving UI States.

> 有关通过配置更改保持简单数据的具体细节，请参阅活动生命周期的保存和恢复活动状态部分。有关保持简单和复杂UI数据的方法的更多信息，请参阅保存UI状态.

##### Handling multi-window cases(处理多窗口情况)

When an app enters multi-window mode, available in Android 7.0 (API level 24)and higher, the system notifies the currently running activity of a configuration change, thus going through the lifecycle transitions described above. This behavior also occurs if an app already in multi-window mode gets resized. Your activity can handle the configuration change itself, or it can allow the system to destroy the activity and recreate it with the new dimensions.

> 当应用程序进入Android 7.0（API级别24）及更高版本可用的多窗口模式时，系统会通知当前正在运行的配置更改活动，从而执行上述生命周期转换。如果应用程序已处于多窗口模式获取调整大小，也会发生此行为。您的活动可以处理配置更改本身，也可以让系统销毁活动并使用新维度重新创建它。

For more information about the multi-window lifecycle, see the Multi-Window Lifecycle section of the Multi-Window Support page.

> 有关多窗口生命周期的更多信息，请参阅多窗口支持页面的多窗口生命周期部分。

In multi-window mode, although there are two apps that are visible to the user, only the one with which the user is interacting is in the foreground and has focus. That activity is in the Resumed state, while the app in the other window is in the Paused state.

> 在多窗口模式下，尽管用户可以看到两个应用程序，但只有用户正在与之交互的应用程序位于前台并且具有焦点。该活动处于恢复状态，而另一个窗口中的应用处于暂停状态。

When the user switches from app A to app B, the system calls onPause() on app A, and onResume() on app B. It switches between these two methods each time the user toggles between apps.

> 当用户从应用A切换到应用B时，系统调用应用A上的onPause（）和应用B上的onResume（）.每当用户在应用程序之间切换时，它会在这两种方法之间切换。

For more detail about multi-windowing, refer to Multi-Window Support.

> 有关多窗口的更多详细信息，请参阅多窗口支持

##### Activity or dialog appears in foreground(活动或对话框出现在前台)

If a new activity or dialog appears in the foreground, taking focus and partially covering the activity in progress, the covered activity loses focus and enters the Paused state. Then, the system calls onResume() on it.

> 如果前台中出现新的活动或对话框，获取焦点并部分覆盖正在进行的活动，则所涵盖的活动失去焦点并进入暂停状态。然后，系统调用onResume（）。

When the covered activity returns to the foreground and regains focus, it calls onResume().

> 当覆盖的活动返回到前台并重新获得焦点时，它会调用onResume（）。

If a new activity or dialog appears in the foreground, taking focus and completely covering the activity in progress, the covered activity loses focus and enters the Stopped state. The system then, in rapid succession, calls onPause() and onStop().

> 如果前台中出现新的活动或对话框，获得焦点并完全覆盖正在进行的活动，则覆盖的活动失去焦点并进入停止状态。然后，系统快速连续地调用onPause（）和onStop（）。

When the same instance of the covered activity comes back to the foreground, the system calls onRestart(), onStart(), and onResume() on the activity. If it is a new instance of the covered activity that comes to the background, the system does not call onRestart(), only calling onStart() and onResume().

> 当被覆盖的活动的同一个实例回到前台时，系统会在活动上调用onRestart（），onStart（）和onResume（）。如果它是涉及后台的被覆盖活动的新实例，则系统不会调用onRestart（），只调用onStart（）和onResume（）.

> Note: When the user taps the Overview or Home button, the system behaves as if the current activity has been completely covered.

> 注意：当用户点击概览或主页按钮时，系统的行为就好像当前活动已被完全覆盖.

##### User taps Back button(用户点击返回按钮)

If an activity is in the foreground, and the user taps the Back button, the activity transitions through the onPause(), onStop(), and onDestroy() callbacks. In addition to being destroyed, the activity is also removed from the back stack.

> 如果某个活动位于前台，并且用户点击后退按钮，则活动将通过onPause（），onStop（）和onDestroy（）回调进行转换。除了被销毁之外，该活动也从后退栈中移除。

It is important to note that, by default, the onSaveInstanceState() callback does not fire in this case. This behavior is based on the assumption that the user tapped the Back button with no expectation of returning to the same instance of the activity. However, you can override the onBackPressed() method to implement some custom behavior, for example a “confirm-quit" dialog.

> 需要注意的是，默认情况下，onSaveInstanceState（）回调在这种情况下不会触发。此行为基于假设用户点击后退按钮而不期望返回到活动的同一实例。但是，您可以重写onBackPressed（）方法来实现一些自定义行为，例如“confirm-quit”对话框。

If you override the onBackPressed() method, we still highly recommend that you invoke super.onBackPressed() from your overridden method. Otherwise the Back button behavior may be jarring to the user.

> 如果您重写onBackPressed（）方法，我们仍强烈建议您从覆盖的方法调用super.onBackPressed（）。否则，后退按钮的行为可能会对用户产生震动(不和谐)。