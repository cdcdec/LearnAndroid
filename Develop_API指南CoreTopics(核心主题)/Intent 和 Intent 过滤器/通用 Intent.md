# 通用 Intent

> https://developer.android.google.cn/guide/components/intents-common.html#Calendar

Intent 用于通过描述您想在某个 Intent 对象中执行的简单操作（如“查看地图”或“拍摄照片”）来启动另一应用中的某个 Activity。 这种 Intent 称作隐式 Intent，因为它并不指定要启动的应用组件，而是指定一项操作并提供执行该操作所需的一些数据。

当您调用 startActivity() 或 startActivityForResult() 并向其传递隐式 Intent 时，系统会 将 Intent 解析为可处理该 Intent 的应用并启动其对应的 Activity。 如果有多个应用可处理 Intent，系统会为用户显示一个对话框，供其选择要使用的应用。

本页面介绍几种可用于执行常见操作的隐式 Intent，按处理 Intent 的应用类型分成不同部分。 此外，每个部分还介绍如何创建 Intent 过滤器来公布您的应用执行相应操作的能力。

> 注意：如果设备上没有可接收隐式 Intent 的应用，您的应用将在调用 startActivity() 时崩溃。如需事先验证是否存在可接收 Intent 的应用，请对 Intent 对象调用 resolveActivity()。如果结果为非空，则至少有一个应用能够处理该 Intent，并且可以安全调用 startActivity()。 如果结果为空，则您不应使用该 Intent。如有可能，您应停用调用该 Intent 的功能。﻿

如果您不熟悉如何创建 Intent 或 Intent 过滤器，应该先阅读 Intent 和 Intent 过滤器。

如需了解如何从开发主机触发本页面上所列的 Intent，请参阅使用 Android 调试桥验证 Intent。

##### Google Voice Actions

Google Voice Actions 会触发本页面上所列的一些 Intent 来响应语音命令。 如需了解详细信息，请参阅 Google Voice Actions 触发的 Intent。

#### 闹钟

#####创建闹铃

如需创建新闹铃，请使用 ACTION_SET_ALARM 操作并使用下文介绍的 extra 指定时间和消息等闹铃详细信息。

> 注：Android 2.3（API 级别 9）及更高版本上只提供了小时、分钟和消息 extra。 其他 extra 是在更新版本的平台上新增的 extra。

###### 操作

ACTION_SET_ALARM

###### 数据 URI

无

###### MIME 类型

无

###### Extra

EXTRA_HOUR：闹铃的小时。
EXTRA_MINUTES：闹铃的分钟。
EXTRA_MESSAGE：用于标识闹铃的自定义消息。
EXTRA_DAYS：一个 ArrayList，其中包括应重复触发该闹铃的每个周日。 每一天都必须使用 Calendar 类中的某个整型值（如 MONDAY）进行声明。对于一次性闹铃，无需指定此 extra。

EXTRA_RINGTONE：一个 content: URI，用于指定闹铃使用的铃声，也可指定 VALUE_RINGTONE_SILENT 以不使用铃声。
如需使用默认铃声，则无需指定此 extra。

EXTRA_VIBRATE：一个布尔型值，用于指定该闹铃触发时是否振动。
EXTRA_SKIP_UI：一个布尔型值，用于指定响应闹铃的应用在设置闹铃时是否应跳过其 UI。 若为 true，则应用应跳过任何确认 UI，直接设置指定的闹铃。

###### 示例 Intent：

```
public void createAlarm(String message, int hour, int minutes) {
    Intent intent = new Intent(AlarmClock.ACTION_SET_ALARM)
            .putExtra(AlarmClock.EXTRA_MESSAGE, message)
            .putExtra(AlarmClock.EXTRA_HOUR, hour)
            .putExtra(AlarmClock.EXTRA_MINUTES, minutes);
    if (intent.resolveActivity(getPackageManager()) != null) {
        startActivity(intent);
    }
}
```

> 注：为了调用 ACTION_SET_ALARM Intent，您的应用必须具有 SET_ALARM 权限：
>
> ```
> <uses-permission android:name="com.android.alarm.permission.SET_ALARM" />
> ```
>
> 

###### 示例 Intent 过滤器：

```
<activity ...>
    <intent-filter>
        <action android:name="android.intent.action.SET_ALARM" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

##### 创建定时器

如需创建倒计时器，请使用 ACTION_SET_TIMER 操作并使用下文介绍的 extra 指定持续时间等定时器详细信息。

> 注：此 Intent 是在 Android 4.4（API 级别 19）中添加的。

操作:ACTION_SET_TIMER

数据URI:无

MIME类型：无

Extra：

EXTRA_LENGTH------>以秒为单位的定时器定时长度。

EXTRA_MESSAGE------>用于标识定时器的自定义消息。

EXTRA_SKIP_UI------>一个布尔型值，用于指定响应定时器的应用在设置定时器时是否应跳过其 UI。 若为 true，则应用应跳过任何确认 UI，直接启动指定的定时器。

##### 示例 Intent：

```
public void startTimer(String message, int seconds) {
    Intent intent = new Intent(AlarmClock.ACTION_SET_TIMER)
            .putExtra(AlarmClock.EXTRA_MESSAGE, message)
            .putExtra(AlarmClock.EXTRA_LENGTH, seconds)
            .putExtra(AlarmClock.EXTRA_SKIP_UI, true);
    if (intent.resolveActivity(getPackageManager()) != null) {
        startActivity(intent);
    }
}
```

>注：为了调用 ACTION_SET_TIMER Intent，您的应用必须具有 SET_ALARM 权限：
>
>```
><uses-permission android:name="com.android.alarm.permission.SET_ALARM" />
>```
>
>

##### 示例 Intent 过滤器：

```
<activity ...>
    <intent-filter>
        <action android:name="android.intent.action.SET_TIMER" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

##### 显示所有闹铃

如需显示闹铃列表，请使用 ACTION_SHOW_ALARMS 操作。

尽管调用此 Intent 的应用并不多（使用它的主要是系统应用），但任何充当闹钟的应用都应实现此 Intent 过滤器，并通过显示现有闹铃列表作出响应。

> 注：此 Intent 是在 Android 4.4（API 级别 19）中添加的。

操作:ACTION_SHOW_ALARMS

数据URI:无

MIME类型：无

###### 示例 Intent 过滤器：

```
<activity ...>
    <intent-filter>
        <action android:name="android.intent.action.SHOW_ALARMS" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

#### 日历

##### 添加日历事件

如需向用户的日历添加新事件，请使用 ACTION_INSERT 操作指定具有 Events.CONTENT_URI 的数据 URI。 然后您就可以使用下文介绍的 extra 指定事件的各类详细信息。

操作:ACTION_INSERT

数据URI:Events.CONTENT_URI

MIME类型："vnd.android.cursor.dir/event"

Extra:

EXTRA_EVENT_ALL_DAY---------->一个布尔型值，指定此事件是否为全天事件。

EXTRA_EVENT_BEGIN_TIME---------->事件的开始时间（从新纪年开始计算的毫秒数）。

EXTRA_EVENT_END_TIME---------->事件的结束时间（从新纪年开始计算的毫秒数）。

TITLE---------->事件标题。

DESCRIPTION---------->事件说明。

EVENT_LOCATION---------->事件地点。

EXTRA_EMAIL---------->以逗号分隔的受邀者电子邮件地址列表。

可使用 CalendarContract.EventsColumns 类中定义的常量指定许多其他事件详细信息。

###### 示例 Intent：

```
public void addEvent(String title, String location, Calendar begin, Calendar end) {
    Intent intent = new Intent(Intent.ACTION_INSERT)
            .setData(Events.CONTENT_URI)
            .putExtra(Events.TITLE, title)
            .putExtra(Events.EVENT_LOCATION, location)
            .putExtra(CalendarContract.EXTRA_EVENT_BEGIN_TIME, begin)
            .putExtra(CalendarContract.EXTRA_EVENT_END_TIME, end);
    if (intent.resolveActivity(getPackageManager()) != null) {
        startActivity(intent);
    }
}
```

###### 示例 Intent 过滤器：

```
<activity ...>
    <intent-filter>
        <action android:name="android.intent.action.INSERT" />
        <data android:mimeType="vnd.android.cursor.dir/event" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

