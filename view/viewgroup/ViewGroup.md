## ViewGroup

#### 继承体系
android.view.ViewGroup-->View

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

