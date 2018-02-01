## NavigationView

#### 继承体系
android.support.design.widget.NavigationView-->ScrimInsetsFrameLayout-->FrameLayout-->ViewGroup-->View

#### 常用方法

```
<android.support.design.widget.NavigationView
android:id="@+id/navigation_view"
android:layout_width="wrap_content"
android:layout_height="match_parent"
android:layout_gravity="start"
android:fitsSystemWindows="true"
app:headerLayout="@layout/navigation_drawer_header"//左侧抽屉上部内容
app:menu="@menu/navigation_drawer_menu" />//左侧抽屉内容
```

1.setItemIconTintList(@Nullable ColorStateList tint)，为null时,设置菜单图标恢复本来的颜色

2.setNavigationItemSelectedListener(@Nullable OnNavigationItemSelectedListener listener),Set a listener that will be notified when a menu item is selected.