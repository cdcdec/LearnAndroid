## NotificationCompat.Builder

#### 继承体系
android.support.v4.app.NotificationCompat.Builder-->java.lang.Object



Builder class for NotificationCompat objects. Allows easier control over all the flags, as well as help constructing the typical notification layouts.On platform versions that don't offer expanded notifications, methods that depend on expanded notifications have no effect.

For example, action buttons won't appear on platforms prior to Android 4.1.Action buttons depend on expanded notifications, which are only available in Android 4.1 and later.

For this reason, you should always ensure that UI controls in a notification are also available in an Activity in your app, and you should always start that Activity when users click the notification.

To do this, use the setContentIntent() method.





NotificationCompat对象的生成器类。允许更轻松地控制所有标志，并帮助构建典型的通知布局。在不提供扩展通知的平台版本上，依赖扩展通知的方法不起作用。例如，操作按钮不会出现在Android 4.1之前的平台上。操作按钮取决于扩展通知，这些通知仅适用于Android 4.1及更高版本。

出于这个原因，您应该始终确保通知中的UI控件也可以在应用中的Activity中使用，并且您应该始终在用户单击通知时启动该Activity。为此，请使用setContentIntent（）方法。



#### 常用方法
1.setDescendantFocusability(int focusability),

```
Set the descendant(后裔; 后代; 派生物) focusability of this view group.This defines the relationship between this view group and its descendants when looking for a view to
take focus in {@link #requestFocus(int, android.graphics.Rect)}.

int focusability的可能取值：
1.FOCUS_BEFORE_DESCENDANTS（This view will get focus before any of its descendants.）
2.FOCUS_AFTER_DESCENDANTS(This view will get focus only if none of its descendants want it.)
3.FOCUS_BLOCK_DESCENDANTS(This view will block any of its descendants from getting focus, even if they are focusable.)
```

