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

| Category | Methods      | Description                                                  |
| -------- | ------------ | ------------------------------------------------------------ |
| Creation | Constructors | There is a form of the constructor that are called when the view is created from code and a form that is called when the view is inflated from a layout file. The second form should parse and apply any attributes defined in the layout file. |
|          |              |                                                              |
|          |              |                                                              |
|          |              |                                                              |
|          |              |                                                              |
|          |              |                                                              |

