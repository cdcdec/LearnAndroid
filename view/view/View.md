## View

#### 继承体系
android.view.View-->Object

This class represents the basic building block for user interface components. A View occupies a rectangular area on the screen and is responsible for drawing and event handling. View is the base class for widgets, which are used to create interactive UI components (buttons, text fields, etc.). The ViewGroup subclass is the base class for layouts, which are invisible containers that hold other Views (or other ViewGroups) and define their layout properties.

> 该类表示用户界面组件的基本构建块。视图占用屏幕上的矩形区域，负责绘图和事件处理。View是小部件的基类，用于创建交互式UI组件（按钮，文本字段等）。ViewGroup子类是布局的基类，它是不可见的容器，可容纳其他视图（或其他视图组）并定义其布局属性。

##### Developer Guides

For information about using this class to develop your application's user interface, read the User Interface developer guide.

> 有关使用此类来开发应用程序的用户界面的信息，请阅读用户界面开发人员指南。

##### Using Views(使用视图)

All of the views in a window are arranged in a single tree. You can add views either from code or by specifying a tree of views in one or more XML layout files. There are many specialized subclasses of views that act as controls or are capable of displaying text, images, or other content.

> 窗口中的所有视图都排列在一棵树中。您可以从代码或通过在一个或多个XML布局文件中指定视图树来添加视图。有许多特殊的视图子类可用作控件或能够显示文本，图像或其他内容。

Once you have created a tree of views, there are typically a few types of common operations you may wish to perform:

> 一旦你创建了一个视图树，通常你可能希望执行几种常见的操作：

* Set properties: for example setting the text of a TextView. The available properties and the methods that set them will vary among the different subclasses of views. Note that properties that are known at build time can be set in the XML layout files.

  > 设置属性:例如设置TextView的文本。可用的属性和设置它们的方法在视图的不同子类中会有所不同。请注意，构建时已知的属性可以在XML布局文件中设置。

* Set focus: The framework will handle moving focus in response to user input. To force focus to a specific view, call requestFocus().

  > 设置焦点：框架将响应用户输入来处理移动焦点。要将焦点强制到特定视图，请调用requestFocus（）。

* Set up listeners: Views allow clients to set listeners that will be notified when something interesting happens to the view. For example, all views will let you set a listener to be notified when the view gains or loses focus. You can register such a listener using setOnFocusChangeListener(android.view.View.OnFocusChangeListener). Other view subclasses offer more specialized listeners. For example, a Button exposes a listener to notify clients when the button is clicked.

  > 设置监听:视图允许客户端设置监听器，当发生相应事件时这些监听器将得到通知。例如，所有视图都会让您设置侦听器，以便在视图获得或失去焦点时得到通知。你可以使用setOnFocusChangeListener（android.view.View.OnFocusChangeListener）来注册这样一个监听器。其他视图子类提供更专业的监听器。例如，一个Button暴露了一个监听器，当按钮被点击时通知客户端。

* Set visibility: You can hide or show views using setVisibility(int).

  > 设置可见性：您可以使用setVisibility（int）隐藏或显示视图。

> Note: The Android framework is responsible for measuring, laying out and drawing views. You should not call methods that perform these actions on views yourself unless you are actually implementing a ViewGroup.
>
> 注意：Android框架负责测量，布局和绘制视图。除非实际实现ViewGroup，否则不应该调用对视图执行这些操作的方法。

#### Implementing a Custom View(实现自定义视图)

To implement a custom view, you will usually begin by providing overrides for some of the standard methods that the framework calls on all views. You do not need to override all of these methods. In fact, you can start by just overriding onDraw(android.graphics.Canvas).

> 要实现一个自定义视图，通常首先要为框架在所有视图上调用的一些标准方法提供重写。您不需要重写所有这些方法。事实上，你可以从覆盖onDraw（android.graphics.Canvas）开始。

| Category         | Methods                                             | Description                                                  |
| ---------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| Creation         | Constructors                                        | There is a form of the constructor that are called when the view is created from code and a form that is called when the view is inflated from a layout file. The second form should parse and apply any attributes defined in the layout file.有一种构造函数的形式，当从代码创建视图时调用该构造函数，并在从布局文件膨胀视图时调用该窗体。第二种形式应解析和应用布局文件中定义的任何属性。 |
| Creation         | onFinishInflate()                                   | Called after a view and all of its children has been inflated from XML.在一个视图及其所有孩子被XML膨胀后调用。 |
| Layout           | onMeasure(int, int)                                 | Called to determine the size requirements for this view and all of its children.呼吁确定此视图及其所有子项的大小要求。 |
| Layout           | onLayout(boolean, int, int, int, int)               | Called when this view should assign a size and position to all of its children.当此视图应为其所有子女分配大小和位置时调用。 |
| Layout           | onSizeChanged(int, int, int, int)                   | Called when the size of this view has changed.当此视图的大小发生变化时调用。 |
| Drawing          | onDraw(android.graphics.Canvas)                     | Called when the view should render its content.当视图应该呈现其内容时调用。 |
| Event processing | onKeyDown(int, KeyEvent)                            | Called when a new hardware key event occurs.当发生新的硬件按键事件时调用。 |
| Event processing | onKeyUp(int, KeyEvent)                              | Called when a hardware key up event occurs.当发生硬件按键事件时调用。 |
| Event processing | onTrackballEvent(MotionEvent)                       | Called when a trackball motion event occurs.当发生轨迹球运动事件时调用。 |
| Event processing | onTouchEvent(MotionEvent)                           | Called when a touch screen motion event occurs.当触摸屏幕运动事件发生时调用。 |
| Focus            | onFocusChanged(boolean, int, android.graphics.Rect) | Called when the view gains or loses focus.当视图获得或失去焦点时调用。 |
| Focus            | onWindowFocusChanged(boolean)                       | Called when the window containing the view gains or loses focus.Called when the window containing the view gains or loses focus. |
| Attaching        | onAttachedToWindow()                                | Called when the view is attached to a window.当视图连接到窗口时调用。 |
| Attaching        | onDetachedFromWindow()                              | Called when the view is detached from its window.当视图从窗口分离时调用。 |
| Attaching        | onWindowVisibilityChanged(int)                      | Called when the visibility of the window containing the view has changed.当包含视图的窗口的可见性发生变化时调用。 |

##### IDs

Views may have an integer id associated with them. These ids are typically assigned in the layout XML files, and are used to find specific views within the view tree. A common pattern is to:

> 视图可能有一个与它们相关的整数ID。这些ID通常分配在布局XML文件中，并用于查找视图树中的特定视图。一个常见的模式是:

* Define a Button in the layout file and assign it a unique ID.在布局文件中定义一个按钮并为其分配一个唯一的ID。

```
<Button
     android:id="@+id/my_button"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:text="@string/my_button_text"/>
```

* From the onCreate method of an Activity, find the Button

  ```
  Button myButton = findViewById(R.id.my_button);
  ```

  View IDs need not be unique throughout the tree, but it is good practice to ensure that they are at least unique within the part of the tree you are searching.

  > 视图ID在整个树中不必是唯一的，但确保它们在搜索树的部分内至少是唯一的。

##### Position

The geometry of a view is that of a rectangle. A view has a location, expressed as a pair of left and top coordinates, and two dimensions, expressed as a width and a height. The unit for location and dimensions is the pixel.

> 视图的几何图形是矩形的几何图形。视图有一个位置，表示为一对左和顶坐标，以及两个维度，表示为宽度和高度。位置和尺寸的单位是像素。

It is possible to retrieve the location of a view by invoking the methods getLeft() and getTop(). The former returns the left, or X, coordinate of the rectangle representing the view. The latter returns the top, or Y, coordinate of the rectangle representing the view. These methods both return the location of the view relative to its parent. For instance, when getLeft() returns 20, that means the view is located 20 pixels to the right of the left edge of its direct parent.

> 通过调用getLeft（）和getTop（）方法可以检索视图的位置。前者返回表示视图的矩形的左边或X坐标。后者返回表示视图的矩形的顶部或Y坐标。这些方法都返回视图相对于其父项的位置。例如，当getLeft（）返回20时，这意味着视图位于其直接父级左边缘右侧20个像素处。

In addition, several convenience methods are offered to avoid unnecessary computations, namely getRight() and getBottom(). These methods return the coordinates of the right and bottom edges of the rectangle representing the view. For instance, calling getRight() is similar to the following computation: getLeft() + getWidth() (see Size for more information about the width.)

> 另外，还提供了几种方便的方法来避免不必要的计算，即getRight（）和getBottom（）。这些方法返回代表视图的矩形右边和底边的坐标。例如，调用getRight（）与以下计算类似：getLeft（）+ getWidth（）（有关宽度的更多信息，请参阅Size。）

#### Size, padding and margins(大小，填充和边距)

The size of a view is expressed with a width and a height. A view actually possess two pairs of width and height values.

> 视图的大小用宽度和高度表示。一个视图实际上拥有两对宽度和高度值。

The first pair is known as measured width and measured height. These dimensions define how big a view wants to be within its parent (see Layout for more details.) The measured dimensions can be obtained by calling getMeasuredWidth() and getMeasuredHeight().

> 第一对称为测量宽度和测量高度。这些维度定义了视图想要在其父代中有多大（有关更多详细信息，请参阅布局）。测量的尺寸可以通过调用getMeasuredWidth（）和getMeasuredHeight（）来获得。

The second pair is simply known as width and height, or sometimes drawing width and drawing height. These dimensions define the actual size of the view on screen, at drawing time and after layout. These values may, but do not have to, be different from the measured width and height. The width and height can be obtained by calling getWidth() and getHeight().

> 第二对简单地称为宽度和高度，或者有时绘制宽度和绘图高度。这些尺寸定义了屏幕上的视图的实际尺寸，在绘制时和布局之后。这些值可能但不一定与测量的宽度和高度不同。宽度和高度可以通过调用getWidth（）和getHeight（）来获得。

To measure its dimensions, a view takes into account its padding. The padding is expressed in pixels for the left, top, right and bottom parts of the view. Padding can be used to offset the content of the view by a specific amount of pixels. For instance, a left padding of 2 will push the view's content by 2 pixels to the right of the left edge. Padding can be set using the setPadding(int, int, int, int) or setPaddingRelative(int, int, int, int) method and queried by calling getPaddingLeft(), getPaddingTop(), getPaddingRight(), getPaddingBottom(), getPaddingStart(), getPaddingEnd().

> 为了测量其尺寸，视图考虑了其填充。填充用视图左侧，顶部，右侧和底部的像素表示。填充可用于将视图的内容偏移特定量的像素。例如，左边距2会将视图的内容向左边缘的右侧推动2个像素。可以使用setPadding（int，int，int，int）或setPaddingRelative（int，int，int，int）方法设置填充，并通过调用getPaddingLeft（），getPaddingTop（），getPaddingRight（），getPaddingBottom（），getPaddingStart ），getPaddingEnd（）。

Even though a view can define a padding, it does not provide any support for margins. However, view groups provide such a support. Refer to ViewGroup and ViewGroup.MarginLayoutParams for further information.

> 尽管视图可以定义填充，但它不提供对边距的支持。但是view groups，提供了这样的支持。有关更多信息，请参阅ViewGroup和ViewGroup.MarginLayoutParams。

#### Layout

Layout is a two pass process: a measure pass and a layout pass. The measuring pass is implemented in measure(int, int) and is a top-down traversal of the view tree. Each view pushes dimension specifications down the tree during the recursion. At the end of the measure pass, every view has stored its measurements. The second pass happens in layout(int, int, int, int) and is also top-down. During this pass each parent is responsible for positioning all of its children using the sizes computed in the measure pass.

> 布局是一个两步过程：测量过程和布局过程。测量过程在measure（int，int）中实现，是视图树的自顶向下遍历。在递归过程中，每个视图都会在树中向下推维度规范。在测量结束时，每个视图都存储了它的测量结果。第二遍发生在布局（int，int，int，int）并且也是自顶向下的。在此期间，每位家长都有责任使用度量关卡中计算的尺寸来定位其所有孩子。

When a view's measure() method returns, its getMeasuredWidth() and getMeasuredHeight() values must be set, along with those for all of that view's descendants. A view's measured width and measured height values must respect the constraints imposed by the view's parents. This guarantees that at the end of the measure pass, all parents accept all of their children's measurements. A parent view may call measure() more than once on its children. For example, the parent may measure each child once with unspecified dimensions to find out how big they want to be, then call measure() on them again with actual numbers if the sum of all the children's unconstrained sizes is too big or too small.

> 当视图的measure（）方法返回时，必须设置它的getMeasuredWidth（）和getMeasuredHeight（）值，以及所有该视图的后代的值。视图的测量宽度和测量的高度值必须尊重视图父母施加的约束。这保证了在测量结束时，所有父母都接受他们所有孩子的测量。父视图可能会多次调用measure（）对其子项。例如，父母可以用未指定的维度测量每个孩子一次，以确定他们想要的大小，然后如果所有孩子的不受约束的大小的总和过大或过小，则再次用实际数字对它们调用measure（）。

The measure pass uses two classes to communicate dimensions. The View.MeasureSpec class is used by views to tell their parents how they want to be measured and positioned. The base LayoutParams class just describes how big the view wants to be for both width and height. For each dimension, it can specify one of:

> 测量过程使用两个类来传达维度。View.MeasureSpec类被视图用来告诉他们的父母他们想要被测量和定位。基本的LayoutParams类只是描述了视图对于宽度和高度都有多大。对于每个维度，它可以指定以下之一：

* an exact number (一个确切的数字)
* MATCH_PARENT, which means the view wants to be as big as its parent (minus padding),  MATCH_PARENT，这意味着该视图要像其父项一样大（减去填充）
* WRAP_CONTENT, which means that the view wants to be just big enough to enclose its content (plus padding).     WRAP_CONTENT，这意味着视图要足够大以包含其内容（加上填充）。

There are subclasses of LayoutParams for different subclasses of ViewGroup. For example, AbsoluteLayout has its own subclass of LayoutParams which adds an X and Y value.

> ViewGroup的不同子类有LayoutParams的子类。例如，AbsoluteLayout有它自己的LayoutParams的子类，它增加了一个X和Y值。

MeasureSpecs are used to push requirements down the tree from parent to child. A MeasureSpec can be in one of three modes:

> MeasureSpecs用于将需求从父级推送到子级。MeasureSpec可以处于三种模式之一:

* UNSPECIFIED: This is used by a parent to determine the desired dimension of a child view. For example, a LinearLayout may call measure() on its child with the height set to UNSPECIFIED and a width of EXACTLY 240 to find out how tall the child view wants to be given a width of 240 pixels.

  > 这由父级用来确定子视图的期望维度。例如，一个LinearLayout可以在它的子级上调用measure（），高度设置为“UNSPECIFIED ”并且宽度为“精确”240，以查找子视图想要赋予240像素宽度的高度。

* EXACTLY: This is used by the parent to impose an exact size on the child. The child must use this size, and guarantee that all of its descendants will fit within this size.

  > 这由家长使用，以确定孩子的确切身高。孩子必须使用这个尺寸，并保证所有的后代都符合这个尺寸。

* AT_MOST: This is used by the parent to impose a maximum size on the child. The child must guarantee that it and all of its descendants will fit within this size.

  > 这由家长使用，以对孩子施加最大尺寸。孩子必须保证它和它的所有后代都适合这个尺寸。

To initiate a layout, call requestLayout(). This method is typically called by a view on itself when it believes that is can no longer fit within its current bounds.

> 要启动布局，请调用requestLayout（）。当它认为不再适合其当前边界时，通常通过对其本身的视图来调用该方法。

##### Drawing

Drawing is handled by walking the tree and recording the drawing commands of any View that needs to update. After this, the drawing commands of the entire tree are issued to screen, clipped to the newly damaged area.

> 通过走树并记录需要更新的任何视图的绘图命令来处理绘图。在此之后，整个树的绘图命令被发布到屏幕上，并被剪裁到新损坏的区域。

The tree is largely recorded and drawn in order, with parents drawn before (i.e., behind) their children, with siblings drawn in the order they appear in the tree. If you set a background drawable for a View, then the View will draw it before calling back to its onDraw() method. The child drawing order can be overridden with custom child drawing order in a ViewGroup, and with setZ(float) custom Z values} set on Views.

> 树大部分被记录和绘制，父母在子女之前（即在他们的后面）绘制，并且兄弟姐妹按照它们出现在树中的顺序绘制。如果你为一个View设置了一个可绘制的背景，那么这个View就会在绘制它的onDraw（）方法之前绘制它。可以使用ViewGroup中的自定义子绘图顺序以及在Views上设置setZ（float）自定义Z值来覆盖子绘图顺序。

To force a view to draw, call invalidate().

> 要强制视图绘制，请调用invalidate（）。

#### Event Handling and Threading(事件处理和线程处理)

The basic cycle of a view is as follows:

> 一个view的基本周期如下：

1.An event comes in and is dispatched to the appropriate view. The view handles the event and notifies any listeners.

> 一个事件进入并被分派到适当的视图。视图处理事件并通知任何监听者。

2.If in the course of processing the event, the view's bounds may need to be changed, the view will call requestLayout().

> 如果在处理事件的过程中，视图的边界可能需要更改，视图将调用requestLayout（）。

3.Similarly, if in the course of processing the event the view's appearance may need to be changed, the view will call invalidate().

> 同样，如果在处理事件的过程中视图的外观可能需要更改，视图将调用invalidate（）。

4.If either requestLayout() or invalidate() were called, the framework will take care of measuring, laying out, and drawing the tree as appropriate.

> 如果调用了requestLayout（）或invalidate（），框架将会适当地处理测量，布局和绘制树。

Note: The entire view tree is single threaded. You must always be on the UI thread when calling any method on any view. If you are doing work on other threads and want to update the state of a view from that thread, you should use a Handler.

> 整个视图树是单线程的。在任何视图中调用任何方法时，您都必须在UI线程上。如果你正在其他线程上工作，并想从该线程更新视图的状态，则应使用Handler。

##### Focus Handling(焦点处理)

The framework will handle routine focus movement in response to user input. This includes changing the focus as views are removed or hidden, or as new views become available. Views indicate their willingness to take focus through the isFocusable() method. To change whether a view can take focus, call setFocusable(boolean). When in touch mode (see notes below) views indicate whether they still would like focus via isFocusableInTouchMode() and can change this via setFocusableInTouchMode(boolean).

> 该框架将响应用户输入来处理常规焦点移动。这包括在视图被移除或隐藏时或在新视图可用时更改焦点。视图表明他们愿意通过isFocusable（）方法来关注焦点。要更改视图是否可以关注焦点，请调用setFocusable（boolean）。当处于触摸模式（请参阅下面的注释）时，视图指示他们是否仍然希望通过isFocusableInTouchMode（）进行聚焦，并且可以通过setFocusableInTouchMode（boolean）更改此属性。

Focus movement is based on an algorithm which finds the nearest neighbor in a given direction. In rare cases, the default algorithm may not match the intended behavior of the developer. In these situations, you can provide explicit overrides by using these XML attributes in the layout file:

> 焦点运动基于在给定方向上找到最近邻居的算法。在极少数情况下，默认算法可能与开发人员的预期行为不匹配。在这些情况下，您可以通过在布局文件中使用这些XML属性来提供显式覆盖:

```
nextFocusDown
 nextFocusLeft
 nextFocusRight
 nextFocusUp
```

To get a particular view to take focus, call requestFocus().

> 要获得特定的视图以获得焦点，请调用requestFocus（）。

##### Touch Mode(触摸模式)

When a user is navigating a user interface via directional keys such as a D-pad, it is necessary to give focus to actionable items such as buttons so the user can see what will take input. If the device has touch capabilities, however, and the user begins interacting with the interface by touching it, it is no longer necessary to always highlight, or give focus to, a particular view. This motivates a mode for interaction named 'touch mode'.

> 当用户通过方向键（例如D-pad）导航用户界面时，有必要将焦点放在可操作的项目上，例如按钮，以便用户可以看到将要输入的内容。但是，如果设备具有触摸功能，并且用户通过触摸界面开始与界面进行交互，则不再需要始终突出显示或关注特定视图。这激发了一种名为“触摸模式”的互动模式。

For a touch capable device, once the user touches the screen, the device will enter touch mode. From this point onward, only views for which isFocusableInTouchMode() is true will be focusable, such as text editing widgets. Other views that are touchable, like buttons, will not take focus when touched; they will only fire the on click listeners.

> 对于支持触摸的设备，一旦用户触摸屏幕，设备将进入触摸模式。从这一点开始，只有isFocusableInTouchMode（）才为true的视图才是可以聚焦的，例如文本编辑小部件。其他可触摸的视图（如按钮）在触摸时不会占用焦点;他们只会点击点击侦听器。无论用户何时点击方向键（如D-pad方向），视图设备都将退出触摸模式，并找到一个视图进行聚焦，以便用户可以继续与用户界面进行交互，而无需再次触摸屏幕。

The touch mode state is maintained across Activitys. Call isInTouchMode() to see whether the device is currently in touch mode.

> 触摸模式状态在各个活动(Activity)中保持不变。调用isInTouchMode（）查看设备当前是否处于触摸模式。

##### Scrolling(滚动)

The framework provides basic support for views that wish to internally scroll their content. This includes keeping track of the X and Y scroll offset as well as mechanisms for drawing scrollbars. See scrollBy(int, int), scrollTo(int, int), and awakenScrollBars() for more details.

> 该框架为希望内部滚动其内容的视图提供基本支持。这包括跟踪X和Y滚动偏移量以及绘制滚动条的机制。有关更多详细信息，请参阅scrollBy（int，int），scrollTo（int，int）和awakenScrollBars（）。

##### Tags(标签)

Unlike IDs, tags are not used to identify views. Tags are essentially an extra piece of information that can be associated with a view. They are most often used as a convenience to store data related to views in the views themselves rather than by putting them in a separate structure.

> 与ID不同，标签不用于识别视图。标签本质上是一个额外的信息，可以与视图相关联。它们通常用于方便地在视图中存储与视图相关的数据，而不是将它们放入单独的结构中。

Tags may be specified with character sequence values in layout XML as either a single tag using the android:tag attribute or multiple tags using the \<tag\> child element:

> 标签可以用布局XML中的字符序列值指定为使用android：tag属性的单个标签或使用\<tag\>子元素的多个标签：

```
 <View ...
           android:tag="@string/mytag_value" />
     <View ...>
         <tag android:id="@+id/mytag"
              android:value="@string/mytag_value" />
     </View>
```

Tags may also be specified with arbitrary objects from code using setTag(Object) or setTag(int, Object).

> 也可以使用setTag（Object）或setTag（int，Object）通过代码中的任意对象指定标签。

##### Themes(主题)

By default, Views are created using the theme of the Context object supplied to their constructor; however, a different theme may be specified by using the android:theme attribute in layout XML or by passing a ContextThemeWrapper to the constructor from code.

> 默认情况下，视图是使用提供给其构造函数的Context对象的主题创建的;然而，可以通过在布局XML中使用android：theme属性或通过从代码传递ContextThemeWrapper到构造函数来指定不同的主题。

When the android:theme attribute is used in XML, the specified theme is applied on top of the inflation context's theme (see LayoutInflater) and used for the view itself as well as any child elements.

> 当在XML中使用android：theme属性时，指定的主题将应用于通胀上下文的主题之上（请参阅LayoutInflater），并用于视图本身以及任何子元素。

In the following example, both views will be created using the Material dark color scheme; however, because an overlay theme is used which only defines a subset of attributes, the value of android:colorAccent defined on the inflation context's theme (e.g. the Activity theme) will be preserved.

> 在下面的例子中，两个视图都将使用材质深色配色方案创建;然而，因为使用仅定义属性子集的覆盖主题，所以在通货膨胀上下文的主题（例如活动主题）上定义的android：colorAccent的值将被保留。

```
<LinearLayout
             ...
             android:theme="@android:theme/ThemeOverlay.Material.Dark">
         <View ...>
     </LinearLayout>
```

##### Properties(属性)

The View class exposes an ALPHA property, as well as several transform-related properties, such as TRANSLATION_X and TRANSLATION_Y. These properties are available both in the Property form as well as in similarly-named setter/getter methods (such as setAlpha(float) for ALPHA). These properties can be used to set persistent state associated with these rendering-related properties on the view. The properties and methods can also be used in conjunction with Animator-based animations, described more in the Animation section.

> View类公开了ALPHA属性以及几个与变换相关的属性，如TRANSLATION_X和TRANSLATION_Y。这些属性在Property表单中以及类似命名的setter / getter方法（例如ALPHA的setAlpha（float））中都可用。这些属性可用于在视图上设置与这些与渲染相关的属性相关的持久状态。这些属性和方法也可以与基于动画的动画结合使用，在动画部分有更多描述。

##### Security

Sometimes it is essential that an application be able to verify that an action is being performed with the full knowledge and consent of the user, such as granting a permission request, making a purchase or clicking on an advertisement. Unfortunately, a malicious application could try to spoof the user into performing these actions, unaware, by concealing the intended purpose of the view. As a remedy, the framework offers a touch filtering mechanism that can be used to improve the security of views that provide access to sensitive functionality.

> 有时，应用程序必须能够在用户完全了解和同意的情况下验证某项操作是否正在执行，例如授予许可请求，进行购买或点击广告。不幸的是，恶意应用程序可能会试图通过隐藏视图的预期目的来欺骗用户执行这些操作，而不会意识到这一点。作为一种补救措施，该框架提供了一种触摸过滤机制，可用于提高视图的安全性，从而提供对敏感功能的访问。

To enable touch filtering, call setFilterTouchesWhenObscured(boolean) or set the android:filterTouchesWhenObscured layout attribute to true. When enabled, the framework will discard touches that are received whenever the view's window is obscured by another visible window. As a result, the view will not receive touches whenever a toast, dialog or other window appears above the view's window.

> 要启用触摸过滤，请调用setFilterTouchesWhenObscured（boolean）或将android：filterTouchesWhenObscured布局属性设置为true。启用后，框架将放弃每当视图的窗口被另一个可见窗口遮挡时接收到的触摸。因此，无论何时在视图窗口上方显示吐司，对话框或其他窗口，视图都不会接收到触摸。

For more fine-grained control over security, consider overriding the onFilterTouchEventForSecurity(MotionEvent) method to implement your own security policy. See also FLAG_WINDOW_IS_OBSCURED.

> 为了更好地控制安全性，可以考虑重写onFilterTouchEventForSecurity（MotionEvent）方法来实现您自己的安全策略。另请参阅FLAG_WINDOW_IS_OBSCURED。

