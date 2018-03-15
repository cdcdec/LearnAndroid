# Build a Responsive UI with ConstraintLayout

> 使用ConstraintLayout构建响应式UI
>
> https://developer.android.google.cn/training/constraint-layout/index.html﻿

ConstraintLayout allows you to create large and complex layouts with a flat view hierarchy (no nested view groups). It's similar to RelativeLayout in that all views are laid out according to relationships between sibling views and the parent layout, but it's more flexible than RelativeLayout and easier to use with Android Studio's Layout Editor.

> ConstraintLayout允许您使用平面视图层次结构创建大而复杂的布局（不包含嵌套视图组）。它与RelativeLayout类似，所有视图都根据兄弟视图和父布局之间的关系进行布局，但它比RelativeLayout更灵活，并且更易于与Android Studio的布局编辑器一起使用。

All the power of ConstraintLayout is available directly from the Layout Editor's visual tools, because the layout API and the Layout Editor were specially built for each other. So you can build your layout with ConstraintLayout entirely by drag-and-dropping instead of editing the XML.

> ConstraintLayout的所有功能都可以直接从布局编辑器的可视化工具中使用，因为布局API和布局编辑器是专门为对方构建的。因此，您可以使用ConstraintLayout完全通过拖放构建布局，而不是编辑XML。

ConstraintLayout is available in an API library that's compatible with Android 2.3 (API level 9) and higher. This page provides a guide to building a layout with ConstraintLayout in Android Studio 3.0 or higher. If you'd like more information about the Layout Editor itself, see the Android Studio guide to Build a UI with Layout Editor.

> ConstraintLayout可在与Android 2.3（API级别9）及更高版本兼容的API库中提供。本页面提供了在Android Studio 3.0或更高版本中使用ConstraintLayout构建布局的指南。如果您想了解更多关于布局编辑器的信息，请参阅Android Studio指南以使用布局编辑器构建UI。

To see a variety of layouts you can create with ConstraintLayout, check out the Constraint Layout Examples project on GitHub.

> 要查看可以使用ConstraintLayout创建的各种布局，请查看GitHub上的约束布局示例项目.

#### Constraints overview(约束概述)

To define a view's position in ConstraintLayout, you must add at least one horizontal and one vertical constraint for the view. Each constraint represents a connection or alignment to another view, the parent layout, or an invisible guideline. Each constraint defines the view's position along either the vertical or horizontal axis; so each view must have a minimum of one constraint for each axis, but often more are necessary.

> 要在ConstraintLayout中定义视图的位置，您必须为视图添加至少一个水平和垂直约束。每个约束表示与另一个视图的连接或对齐，父布局或不可见的准则。每个约束定义视图沿着垂直或水平轴的位置;所以每个视图对于每个轴必须至少有一个约束，但通常更多是必要的。

When you drop a view into the Layout Editor, it stays where you leave it even if it has no constraints. However, this is only to make editing easier; if a view has no constraints when you run your layout on a device, it is drawn at position [0,0] (the top-left corner).

> 将视图拖放到布局编辑器中时，即使它没有约束，它仍然保留在离开它的位置。但是，这只是为了使编辑更容易;如果视图在设备上运行布局时没有约束条件，它将在[0,0]位置（左上角）绘制。

In figure 1, the layout looks good in the editor, but there's no vertical constraint on view C. When this layout draws on a device, view C horizontally aligns with the left and right edges of view A, but appears at the top of the screen because it has no vertical constraint.

> 在图1中，编辑器中的布局看起来不错，但对视图C没有垂直约束。当这个布局在设备上绘制时，视图C水平地与视图A的左边和右边对齐，但由于它没有垂直约束而出现在屏幕的顶部。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\constraint-fail_2x.png)
Figure 1. The editor shows view C below A, but it has no vertical constraint.
>图1.编辑器在A下面显示视图C，但它没有垂直约束

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\constraint-fail-fixed_2x.png)
Figure 2. View C is now vertically constrained below view A.

> 图2.视图C现在被垂直约束在视图A下方

Although a missing constraint won't cause a compilation error, the Layout Editor indicates missing constraints as an error in the toolbar. To view the errors and other warnings, click Show Warnings and Errors . To help you avoid missing constraints, the Layout Editor can automatically add constraints for you with the Autoconnect and infer constraints features.

> 虽然缺少约束不会导致编译错误，但布局编辑器会将缺少的约束作为工具栏中的错误进行指示。要查看错误和其他警告，请单击显示警告和错误。为了帮助您避免缺少约束，布局编辑器可以使用Autoconnect为您自动添加约束，并推断约束特征。

#### Add ConstraintLayout to your project(将ConstraintLayout添加到您的项目中)

To use ConstraintLayout in your project, proceed as follows:

> 要在项目中使用ConstraintLayout，请按照下列步骤操作：

1.Ensure you have the maven.google.com repository declared in your module-level build.gradle file:

> 1.确保您的模块级build.gradle文件中声明了maven.google.com存储库：

```
repositories {
    maven {
        url 'https://maven.google.com'
    }
}
```

2.Add the library as a dependency in the same build.gradle file:

> 2.在相同的build.gradle文件中将该库作为依赖项添加：

```
dependencies {
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
}
```

3.In the toolbar or sync notification, click Sync Project with Gradle Files.

> 3.在工具栏或同步通知中，点击将项目与Gradle文件同步。

Now you're ready to build your layout with ConstraintLayout.

> 现在你已经准备好用ConstraintLayout构建你的布局了。

#### Convert a layout(转换布局)

To convert an existing layout to a constraint layout, follow these steps:

> 要将现有布局转换为约束布局，请按照下列步骤操作：

1.Open your layout in Android Studio and click the Design tab at the bottom of the editor window.

> 1.在Android Studio中打开布局，然后单击编辑器窗口底部的设计选项卡。

2.In the Component Tree window, right-click the layout and click Convert layout to ConstraintLayout.

> 2.在Component Tree窗口中，右键单击布局，然后单击Convert layout to ConstraintLayout。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\layout-editor-convert-to-constraint_2x.png)
Figure 3. The menu to convert a layout to ConstraintLayout
> 图3.将布局转换为ConstraintLayout的菜单

#### Create a new layout

To start a new constraint layout file, follow these steps:

> 要启动新的约束布局文件，请按照下列步骤操作：

1.In the Project window, click the module folder and then select File > New > XML > Layout XML.

> 1.在项目窗口中，单击模块文件夹，然后选择文件>新建> XML>布局XML。

2.Enter a name for the layout file and enter "android.support.constraint.ConstraintLayout" for the Root Tag.

>  2.输入布局文件的名称并为根标签输入“android.support.constraint.ConstraintLayout”。

3.Click Finish.

> 3.点击完成

#### Add a constraint(添加一个约束)

Start by dragging a view from the Palette window into the editor. When you add a view in a ConstraintLayout, it displays a bounding box with square resizing handles on each corner and circular constraint handles on each side.

> 首先将视图从Palette窗口拖到编辑器中。当你在一个ConstraintLayout中添加一个视图时，它会在每个角上显示一个边界框，其中每个角上都有方形调整大小的控制柄，每边都有一个圆形约束控制柄。

Click the view to select it. Then click-and-hold one of the constraint handles and drag the line to an available anchor point (the edge of another view, the edge of the layout, or a guideline). When you release, the constraint is made, with a default margin separating the two views.

> 点击视图以选择它。然后单击并按住其中一个约束句柄，并将该线拖到可用的锚点（另一个视图的边缘，布局的边缘或guideline）。当你释放时，约束被创建，用两个视图分开的默认边距。

When creating constraints, remember the following rules:

> 创建约束时，请记住以下规则：

* Every view must have at least two constraints: one horizontal and one vertical.

  > 每个视图必须至少有两个约束：一个水平和一个垂直。

* You can create constraints only between a constraint handle and an anchor point that share the same plane. So a vertical plane (the left and right sides) of a view can be constrained only to another vertical plane; and baselines can constrain only to other baselines.

  > 您只能在约束句柄和共享同一平面的锚点之间创建约束。因此，一个视图的垂直平面（左侧和右侧）可以仅限于另一个垂直平面;基线只能限制到其他基线。

* Each constraint handle can be used for just one constraint, but you can create multiple constraints (from different views) to the same anchor point.

  > 每个约束句柄只能用于一个约束，但您可以创建多个约束（从不同的视图）到同一个锚点。

To remove a constraint, select the view and then click the constraint handle. Or remove all the constraints by selecting the view and then clicking Delete Constraints .

> 要移除约束，请选择视图，然后单击约束手柄。或者通过选择视图然后单击删除约束来删除所有约束。

If you add opposing constraints on a view, the constraint lines become squiggly like a spring to indicate the opposing forces, as shown in video 2. The effect is most visible when the view size is set to "fixed" or "wrap content," in which case the view is centered between the constraints. If you instead want the view to stretch its size to meet the constraints, switch the size to "match constraints"; or if you want to keep the current size but move the view so that it is not centered, adjust the constraint bias.

> 如果在视图上添加相反的约束条件，那么约束线会像弹簧一样变得扭曲，以指示对立的力量，如视频2所示。当视图大小设置为“固定”或“包装内容”时，效果最明显，在这种情况下视图位于约束之间。如果您希望视图扩大其大小以满足约束，请将大小切换为“匹配约束”;或者如果要保留当前大小但移动视图以使其不居中，请调整约束偏差。

You can use constraints to achieve different types of layout behavior, as described in the following sections.

> 您可以使用约束来实现不同类型的布局行为，如以下各节所述。

#### Parent position(父母的位置)

Constrain the side of a view to the corresponding edge of the layout.

> 将视图的一侧限制到布局的相应边缘。

In figure 4, the left side of the view is connected to the left edge of the parent layout. You can define the distance from the edge with margin.

> 在图4中，视图的左侧连接到父布局的左侧边缘。您可以定义边距与边距的距离。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\parent-constraint_2x.png)
Figure 4. A horizontal constraint to the parent
> 图4.父级的水平约束

#### Order position(订单位置)

Define the order of appearance for two views, either vertically or horizontally.

> 定义两个视图的外观顺序，垂直或水平。

In figure 5, B is constrained to always be to the right of A, and C is constrained below A. However, these constraints do not imply alignment, so B can still move up and down.

> 在图5中，B被限制为始终在A的右侧，并且C被约束在A以下。但是，这些约束并不意味着对齐，所以B仍然可以上下移动。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\position-constraint_2x.png)
Figure 5. A horizontal and vertical constraint.

> 图5.水平和垂直约束

#### Alignment(对准)

Align the edge of a view to the same edge of another view.

> 将视图的边缘与另一个视图的相同边缘对齐

In figure 6, the left side of B is aligned to the left side of A. If you want to align the view centers, create a constraint on both sides.

> 在图6中，B的左侧与A的左侧对齐。如果您想对齐视图中心，请在两侧创建一个约束。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\alignment-constraint_2x.png)

Figure 6. A horizontal alignment constraint.

> 图6.一个水平对齐约束

You can offset the alignment by dragging the view inward from the constraint. For example, figure 7 shows B with a 24dp offset alignment. The offset is defined by the constrained view's margin.

> 您可以通过从约束中向内拖动视图来抵消对齐。例如，图7显示了具有24dp偏移对齐的B.偏移量由约束视图的边距来定义。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\alignment-constraint-offset_2x.png)

> 图7.偏移水平对齐约束.Figure 7. An offset horizontal alignment constraint

You can also select all the views you want to align, and then click Align in the toolbar to select the alignment type.

> 您还可以选择要对齐的所有视图，然后单击工具栏中的对齐以选择对齐类型。

#### Baseline alignment(基线对齐)

Align the text baseline of a view to the text baseline of another view.

> 将视图的文本基线与另一个视图的文本基线对齐。

In figure 8, the first line of B is aligned with the text in A.

> 在图8中，B的第一行与A中的文字保持一致。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\baseline-constraint_2x.png)

Figure 8. A baseline alignment constraint.

> 图8.一个基线对齐约束.

To create a baseline constraint, select the text view you want to constrain and then click Edit Baseline , which appears below the view. Then click the text baseline and drag the line to another baseline.

> 要创建基线约束，请选择要约束的文本视图，然后单击视图下方出现的编辑基线。然后单击文本基线并将该行拖到另一个基线。

#### Constrain to a guideline(约束指南)

You can add a vertical or horizontal guideline to which you can constrain views, and the guideline will be invisible to app users. You can position the guideline within the layout based on either dp units or percent, relative to the layout's edge.

> 您可以添加可限制视图的垂直或水平 guideline，该 guideline对应用用户不可见。您可以根据相对于版面边缘的dp单位或百分比在布局中定位指南。

To create a guideline, click Guidelines  in the toolbar, and then click either Add Vertical Guideline or Add Horizontal Guideline.

>  要创建guideline，请单击工具栏中的guideline，然后单击添加垂直guideline或添加水平guideline。

Drag the dotted line to reposition it and click the circle at the edge of the guideline to toggle the measurement mode.

> 拖动虚线重新定位它，并单击guideline边缘的圆以切换测量模式。

Figure 9. A view constrained to a guideline.

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\guideline-constraint_2x.png)

> 图9.限制于guideline的视图.

#### Constrain to a barrier(限制到一个障碍)

Similar to a guideline, a barrier is an invisible line that you can constrain views to. Except a barrier does not define its own position; instead, the barrier position moves based on the position of views contained within it. This is useful when you want to constrain a view to the a set of views rather than to one specific view.

> 与guideline类似，屏障是您可以限制视图的无形线条。除了障碍没有定义自己的位置;相反，屏障位置基于其中包含的视图的位置而移动。当您想要将视图限制为一组视图而非一个特定的视图时，这非常有用。

For example, figure 10 shows view C is constrained to the right side of a barrier. The barrier is set to the "end" (or the right side in a left-to-right layout) of both view A and view B. So the barrier moves depending on whether the right side of view A or view B is is farthest right.

> 例如，图10显示视图C被限制在屏障的右侧。屏障被设置为视图A和视图B两者的“结束”（或者以从左到右布局的右侧）。因此，屏障根据视图A或视图B的右侧是否最远而移动对。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\barrier-constraint_2x.png)
Figure 10 . View C is constrained to a barrier, which moves based on the position/size of both view A and view B.

> 图10.视图C被限制在屏障上，该屏障根据视图A和视图B的位置/大小进行移动



To create a barrier, follow these steps:

> 要创建屏障，请按照下列步骤操作：

1.Click Guidelines  in the toolbar, and then click Add Vertical Barrier or Add Horizontal Barrier.

> 1.单击工具栏中的Guidelines，然后单击添加垂直屏障或添加水平屏障。

2.In the Component Tree window, select the views you want inside the barrier and drag them into the barrier component.

> 2.在“组件树”窗口中，在屏障内选择所需的视图并将其拖入屏障组件。

3.Select the barrier from the Component Tree, open the Attributes  window, and then set the barrierDirection.

> 3.从组件树中选择屏障，打开属性窗口，然后设置barrierDirection。

Now you can create a constraint from another view to the barrier.

> 现在，您可以从另一个视图创建一个约束到屏障。

You can also constrain views that are inside the barrier to the barrier. This way, you can ensure that all views in the barrier always align to each other, even if you don't know which view will be the longest or tallest.

> 您还可以限制屏障内部的视图。这样，即使您不知道哪个视图最长或最高，也可以确保屏障中的所有视图始终相互对齐。

You can also include a guideline inside a barrier to ensure a "minimum" position for the barrier.

> 您还可以在屏障内包含指南以确保屏障的“最小”位置。

#### Adjust the constraint bias(调整约束偏差)

When you add a constraint to both sides of a view (and the view size for the same dimension is either "fixed" or "wrap content"), the view becomes centered between the two constraints with a bias of 50% by default. You can adjust the bias by dragging the bias slider in the Attributes window or by dragging the view, as shown in video 5.

> 当您向视图的两侧添加约束（并且同一维度的视图大小是“固定”或“包装内容”）时，视图将变为居中于两个约束之间，默认偏差为50％.您可以通过拖动“属性”窗口中的偏移滑块或拖动视图来调整偏差，如视频5所示。

If you instead want the view to stretch its size to meet the constraints, switch the size to "match constraints".

> 如果您希望视图伸展其大小以满足约束，请将大小切换为“匹配约束”。

#### Adjust the view size(调整视图大小)

You can use the corner handles to resize a view, but this hard codes the size so the view will not resize for different content or screen sizes. To select a different sizing mode, click a view and open the Attributes  window on the right side of the editor.

> 您可以使用拐角手柄来调整视图大小，但是这会对大小进行硬编码，因此视图不会针对不同的内容或屏幕大小调整大小。要选择不同的尺寸模式，请单击视图并打开编辑器右侧的“属性”窗口。

Near the top of the Attributes window is the view inspector, which includes controls for several layout attributes, as shown in figure 10 (this is available only for views in a constraint layout).

> 在Attributes窗口的顶部附近是视图检查器，它包括几个布局属性的控件，如图10所示（这仅适用于约束布局中的视图）。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\layout-editor-properties-callouts_2x.png)
Figure 10. The Attributes window includes controls for 1 size ratio, 2 delete constraint, 3 height/width mode, 4 margins, and 5 constraint bias.

> 图10. Attributes窗口包含1:大小比例，2:删除约束，3:高度/宽度模式，4:边界和5:约束偏差的控件。

You can change the way the height and width are calculated by clicking the symbols indicated with callout 3 in figure 10. These symbols represent the size mode as follows (click the symbol to toggle between these settings):

> 您可以通过单击图10中标注为3的符号来更改高度和宽度的计算方式。这些符号代表大小模式如下（单击符号以在这些设置之间切换）：

* |-----|Fixed: You specify a specific dimension in the text box below or by resizing the view in the editor.

  > 修正：您可以在下面的文本框中指定特定的维度，或者在编辑器中调整视图的大小。

* \>>>:Wrap Content: The view expands only as much as needed to fit its contents.

  > 包裹内容：该视图只根据需要扩展以适应其内容。

* |曲线|: Match Constraints: The view expands as much as possible to meet the constraints on each side (after accounting for the view's margins). However, you can modify that behavior with the following attributes and values (these attributes take effect only when you set the view width to match constraints):

  > 匹配约束：视图尽可能扩展以满足各方的约束（在考虑了视图边距后）.但是，可以使用以下属性和值修改该行为（只有在将视图宽度设置为与约束匹配时，这些属性才会生效）：


  * layout_constraintWidth_default

    * spread:  Expands the view as much as possible to meet the constraints on each side. This is the default behavior.

      > spread:尽可能扩大视图以满足双方的限制.这是默认行为。

    * wrap: Expands the view only as much as needed to fit its contents, but still allows the view to be smaller than that if the constraints require it. So the difference between this and using Wrap Content (above), is that setting the width to Wrap Content forces the width to always exactly match the content width; whereas using Match Constraints with layout_constraintWidth_default set to wrap also allows the view to be smaller than the content width.

      > wrap:只根据需要扩大视图以适应其内容，但仍然允许视图小于如果约束需要它的视图。所以这和使用Wrap Content（上面）的区别，是将宽度设置为包装内容强制宽度始终与内容宽度完全匹配;而使用Match Constraints和layout_constraintWidth_default设置为包装也允许视图小于内容宽度。

  * layout_constraintWidth_min

    This takes a dp dimension for the view's minimum width.

    > 这为视图的最小宽度需要一个dp尺寸。

  * layout_constraintWidth_max

    This takes a dp dimension for the view's maximum width.

    > 这对视图的最大宽度采用dp尺寸。

  However, if the given dimension has only one constraint, then the view expands to fit its contents. Using this mode on either the height or width also allows you to set a size ratio.

  > 但是，如果给定维度只有一个约束，则该视图将展开以适应其内容。在高度或宽度上使用此模式还可以设置尺寸比例。

> Note: You cannot use match_parent for any view in a ConstraintLayout. Instead use "match constraints" (0dp)

> 你不能在ConstraintLayout的任何视图中使用match_parent。而是使用“匹配约束”（0dp）

#### Set size as a ratio(将大小设置为比率)

You can set the view size to a ratio such as 16:9 if at least one of the view dimensions is set to "match constraints" (0dp). To enable the ratio, click Toggle Aspect Ratio Constraint (callout 1 in figure 10), and then enter the width:height ratio in the input that appears.

> 如果至少有一个视图尺寸设置为“匹配约束”（0dp），则可以将视图尺寸设置为16：9等比例。要启用比率，请单击切换长宽比约束（图10中的标注1），然后在显示的输入中输入宽高比。

If both the width and height are set to match constraints, you can click Toggle Aspect Ratio Constraint to select which dimension is based on a ratio of the other. The view inspector indicates which is set as a ratio by connecting the corresponding edges with a solid line.

> 如果宽度和高度都设置为与约束匹配，则可以单击切换长宽比约束来选择基于另一维度的比例的维度。视图检查器通过用实线连接相应边缘来指示将哪个设置为比率。

For example, if you set both sides to "match constraints", click Toggle Aspect Ratio Constraint twice to set the width be a ratio of the height. Now the entire size is dictated by the height of the view (which can be defined in any way) as shown in figure 11.

> 例如：如果将两侧设置为“匹配约束”，请单击切换宽高比约束两次以将宽度设置为高度的比率。现在整个大小由视图的高度决定（可以用任何方式定义），如图11所示。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\layout-editor-ratio-properties_2x.png)

#### Adjust the view margins（调整视图边距）

To ensure that all your views are evenly spaced, click Margin  in the toolbar to select the default margin for each view that you add to the layout. Any change you make to the default margin applies only to the views you add from then on.

> 为确保所有视图的间隔均匀，请单击工具栏中的“边距”以选择添加到布局的每个视图的默认边距。您对默认边距所做的任何更改仅适用于您从此之后添加的视图。

You can control the margin for each view in the Attributes window by clicking the number on the line that represents each constraint (in figure 10, callout 4 shows the bottom margin is set to 28dp).

> 您可以通过单击表示每个约束的线上的数字（在图10中，标注4显示底部边距设置为28dp）来控制“属性”窗口中每个视图的边距。

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\layout-editor-margin-callout_2x.png)

Figure 12. The toolbar's Margin button.

> 图12.工具栏的Margin按钮。

All margins offered by the tool are factors of 8dp to help your views align to Material Design's 8dp square grid recommendations.

> 该工具提供的所有边距均为8dp，以帮助您的视图与Material Design的8dp方形网格建议保持一致。

#### Control linear groups with a chain(用链控制线性组)

A chain is a group of views that are linked to each other with bi-directional position constraints. For example, figure 13 shows two views that both have a constraint to each other, thus creating a horizontal chain.

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\constraint-chain_2x.png)
Figure 13. A chain with just two views

> 图13.只有两个视图的链


> 一个链是一组相互连接的视图，具有双向位置约束。例如，图13显示了两个视图，这两个视图都相互约束，从而创建一个水平链。

A chain allows you to distribute a group of views horizontally or vertically with the following styles (as shown in figure 14):

![](G:\LearnAndroid\Develop_API指南CoreTopics(核心主题)\Layouts\constraint-chain-styles_2x.png)

Figure 14. Examples of each chain style

> 图14.每个链式样例


> 一个链允许你用下列样式水平或垂直分配一组视图（如图14所示）：

红圈1.Spread: The views are evenly distributed (after margins are accounted for). This is the default.

> 1.Spread:views均匀分布（在占边界之后）。这是默认设置。

红圈2.Spread inside: The first and last view are affixed to the constraints on each end of the chain and the rest are evenly distributed.

> 第一个和最后一个视图固定在链条两端的约束上，其余均匀分布。

红圈3.Weighted: When the chain is set to either spread or spread inside, you can fill the remaining space by setting one or more views to "match constraints" (0dp). By default, the space is evenly distributed between each view that's set to "match constraints," but you can assign a weight of importance to each view using the layout_constraintHorizontal_weight and layout_constraintVertical_weight attributes. If you're familiar with layout_weight in a linear layout, this works the same way. So the view with the highest weight value gets the most amount of space; views that have the same weight get the same amount of space.

> 当链条被设置为传播或扩散时，您可以通过设置一个或多个视图来“填充约束”（0dp）来填充剩余空间。默认情况下，空间均匀分布在设置为“匹配约束”的每个视图之间，但您可以使用layout_constraintHorizontal_weight和layout_constraintVertical_weight属性为每个视图指定一个重要权重。如果您熟悉线性布局中的layout_weight，则其工作方式相同。所以重量值最高的视图获得最多的空间;具有相同重量的视图获得相同的空间量。

红圈4.Packed: The views are packed together (after margins are accounted for). You can then adjust the whole chain's bias (left/right or up/down) by changing the chain's head view bias.

> 这些view被包装在一起（在占边界之后）。然后，您可以通过更改链条的头部视图偏差来调整整个链条的偏差（左/右或上/下）。

The chain's "head" view (the left-most view in a horizontal chain and the top-most view in a vertical chain) defines the chain's style in XML. However, you can toggle between spread, spread inside, and packed by selecting any view in the chain and then clicking the chain button   that appears below the view.

> 链条的“头部”视图（水平链中最左边的视图和垂直链中最顶端的视图）用XML定义链条的样式。但是，您可以通过选择链中的任何视图，然后单击视图下方显示的链式按钮，在展开，展开内部和打包之间切换。

To create a chain of views quickly, select them all, right-click one of the views, and then select either Center Horizontally or Center Vertically, to create either a horizontal or vertical chain, respectively (see video 4).

> 要快速创建视图链，选择全部视图，右键单击其中一个视图，然后选择“水平居中”或“垂直居中”，以分别创建水平或垂直链（请参阅视频4）.

A few other things to consider when using chains:

> 使用链条时需要考虑的其他一些事情：

* A view can be a part of both a horizontal and a vertical chain, making it easy to build flexible grid layouts.

  > 视图可以是水平链和垂直链的一部分，从而可以轻松地构建灵活的网格布局。

* A chain works properly only if each end of the chain is constrained to another object on the same axis, as shown in figure 13.

  > 只有当链条的每一端都被约束到同一轴上的另一个对象时，链才能正常工作，如图13所示。

* Although the orientation of a chain is either vertical or horizontal, using one does not align the views in that direction. So be sure you include other constraints to achieve the proper position for each view in the chain, such as alignment constraints.

  > 尽管链条的方向是垂直或水平的，但使用一个方向不会使视图在该方向上对齐。因此，请确保包含其他约束条件以实现链中每个视图的适当位置，例如对齐约束。

#### Automatically create constraints(自动创建约束)

Instead of adding constraints to every view as you place them in the layout, you can move each view into the positions you desire, and then click Infer Constraints  to automatically create constraints.

> 将各个视图放置在布局中时，不必为每个视图添加约束，您可以将每个视图移动到您想要的位置，然后单击“约束约束(工具栏上像魔棒一样的按钮)”以自动创建约束。

Infer Constraints scans the layout to determine the most effective set of constraints for all views. It makes a best effort to constrain the views to their current positions while allowing flexibility. You might need to make some adjustments to be sure the layout responds as you intend for different screen sizes and orientations.

> 推断约束扫描布局以确定所有视图的最有效的一组约束。它尽最大努力将view限制在当前位置，同时保持灵活性。您可能需要进行一些调整，以确保布局响应您打算的不同屏幕尺寸和方向。

Autoconnect is a separate feature that is either on or off. When turned on, it automatically creates two or more constraints for each view as you add them to the layout, but only when appropriate to constrain the view to the parent layout. Autoconnect does not create constraints to other views in the layout.

> Autoconnect是一个独立的功能，可以打开或关闭。打开时，它会在将每个视图添加到布局时自动创建两个或更多个约束，但只有在适合将视图约束到父布局时才会创建。Autoconnect不会为布局中的其他视图创建约束。

Autoconnect is disabled by default. You can enable it by clicking Turn on Autoconnect  in the Layout Editor toolbar.

> Autoconnect默认是禁用的。您可以通过在布局编辑器工具栏中单击"打开自动连接"(一个像磁铁一样的按钮)来启用它。

