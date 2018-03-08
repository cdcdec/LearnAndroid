## Notification

#### 继承体系
android.app.Notification-->java.lang.Object



A class that represents how a persistent notification is to be presented to the user using the NotificationManager.

The Notification.Builder has been added to make it easier to construct Notifications.



Developer Guides
For a guide to creating notifications, read the Status Bar Notifications developer guide.



表示如何使用NotificationManager向用户呈现持久通知的类。Notification.Builder已添加，以便于构建通知。

有关创建通知的指导，请阅读状态栏通知开发人员指南。



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

