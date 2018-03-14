# Tasks and Back Stack(任务和返回堆栈)

> https://developer.android.google.cn/guide/components/activities/tasks-and-back-stack.html

﻿A task is a collection of activities that users interact with when performing a certain job. The activities are arranged in a stack—the back stack)—in the order in which each activity is opened. For example, an email app might have one activity to show a list of new messages. When the user selects a message, a new activity opens to view that message. This new activity is added to the back stack. If the user presses the Back button, that new activity is finished and popped off the stack. The following video provides a good overview of how the back stack works.

> 任务是用户在执行某项工作时与之交互的一系列活动。这些活动排列在一个堆栈中 - 后台堆栈） - 按每个活动的打开顺序排列。例如，电子邮件应用程序可能有一个活动来显示新消息列表。当用户选择一条消息时，会打开一个新的活动来查看该消息。这个新活动被添加到后退堆栈中。如果用户按下“后退”按钮，则新活动结束并弹出堆栈。以下视频很好地概述了后退堆栈的工作原理.

When apps are running simultaneously in a multi-windowed environment, supported in Android 7.0 (API level 24) and higher, the system manages tasks separately for each window; each window may have multiple tasks. The same holds true for Android apps running on Chromebooks: the system manages tasks, or groups of tasks, on a per-window basis.

> 当应用程序在多窗口环境中同时运行时，在Android 7.0（API级别24）及更高版本中受支持时，系统会为每个窗口分别管理任务;每个窗口可能有多个任务。对于在Chromebook上运行的Android应用程序也是如此：系统以每个窗口为基础管理任务或任务组。

The device Home screen is the starting place for most tasks. When the user touches an icon in the app launcher (or a shortcut on the Home screen), that app's task comes to the foreground. If no task exists for the app (the app has not been used recently), then a new task is created and the "main" activity for that app opens as the root activity in the stack.

> 设备主屏幕是大多数任务的起始位置。当用户触摸应用程序启动器中的图标（或主屏幕上的快捷键）时，该应用程序的任务将显示在前台。如果应用程序没有任何任务（应用程序最近未使用），则会创建一个新任务，并且该应用程序的“主要”活动以堆栈中的根活动打开。

When the current activity starts another, the new activity is pushed on the top of the stack and takes focus. The previous activity remains in the stack, but is stopped. When an activity stops, the system retains the current state of its user interface. When the user presses the Back button, the current activity is popped from the top of the stack (the activity is destroyed) and the previous activity resumes (the previous state of its UI is restored). Activities in the stack are never rearranged, only pushed and popped from the stack—pushed onto the stack when started by the current activity and popped off when the user leaves it using the Back button. As such, the back stack operates as a "last in, first out" object structure. Figure 1 visualizes this behavior with a timeline showing the progress between activities along with the current back stack at each point in time.

>当当前活动开始另一个时，新活动被推到堆栈的顶部并且获得焦点。先前的活动保留在堆栈中，但已停止。当活动停止时，系统保留其用户界面的当前状态。当用户按下“后退”按钮时，当前活动从堆栈顶部弹出（活动被销毁），并恢复前一活动（恢复其UI的以前状态）。堆栈中的活动不会重新排列，只能从堆栈中推送并弹出 - 在当前活动启动时推送到堆栈，并在用户使用“后退”按钮离开堆栈时弹出。因此，后退堆栈作为“后进先出”对象结构运行。图1通过一个时间线将这种行为可视化，该时间表显示活动之间的进展情况以及每个时间点当前的回退堆栈。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Activitivies\diagram_backstack.png)
Figure 1. A representation of how each new activity in a task adds an item to the back stack. When the user presses the Back button, the current activity is destroyed and the previous activity resumes.
>图1.任务中每个新活动如何将项添加到后端堆栈的表示。当用户按下“后退”按钮时，当前活动将被销毁并恢复前一活动。

If the user continues to press Back, then each activity in the stack is popped off to reveal the previous one, until the user returns to the Home screen (or to whichever activity was running when the task began). When all activities are removed from the stack, the task no longer exists.

> 如果用户继续按回，则弹出堆栈中的每个活动以显示前一个活动，直到用户返回主屏幕（或任务开始时的任何活动正在运行）。当所有活动从堆栈中移除时，任务不再存在。

A task is a cohesive unit that can move to the "background" when users begin a new task or go to the Home screen, via the Home button. While in the background, all the activities in the task are stopped, but the back stack for the task remains intact—the task has simply lost focus while another task takes place, as shown in figure 2. A task can then return to the "foreground" so users can pick up where they left off. Suppose, for example, that the current task (Task A) has three activities in its stack—two under the current activity. The user presses the Home button, then starts a new app from the app launcher. When the Home screen appears, Task A goes into the background. When the new app starts, the system starts a task for that app (Task B) with its own stack of activities. After interacting with that app, the user returns Home again and selects the app that originally started Task A. Now, Task A comes to the foreground—all three activities in its stack are intact and the activity at the top of the stack resumes. At this point, the user can also switch back to Task B by going Home and selecting the app icon that started that task (or by selecting the app's task from the Recents screen). This is an example of multitasking on Android.

> 任务是一个内聚单元，当用户开始新任务或通过主页按钮进入主屏幕时，该单元可以移动到“后台”。在后台时，任务中的所有活动都会停止，但任务的后退堆栈保持不变 - 当另一项任务发生时，任务完全失去焦点，如图2所示。