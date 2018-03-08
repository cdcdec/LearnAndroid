## ListActivity

#### 继承体系
android.app.Activity-->android.app.ListActivity



An activity that displays a list of items by binding to a data source such as an array or Cursor, and exposes event handlers when the user selects an item.

ListActivity hosts a ListView object that can be bound to different data sources, typically either an array or a Cursor holding query results. Binding, screen layout, and row layout are discussed in the following sections.

##### Screen Layout

ListActivity has a default layout that consists of a single, full-screen list in the center of the screen. However, if you desire, you can customize the screen layout by setting your own view layout with setContentView() in onCreate(). To do this, your own view MUST contain a ListView object with the id "@android:id/list" (or list if it's in code)

Optionally, your custom view can contain another view object of any type to display when the list view is empty. This "empty list" notifier must have an id "android:id/empty". Note that when an empty view is present, the list view will be hidden when there is no data to display.

The following code demonstrates an (ugly) custom screen layout. It has a list with a green background, and an alternate red "no data" message.

```
<?xml version="1.0" encoding="utf-8"?>
 <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:orientation="vertical"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:paddingLeft="8dp"
         android:paddingRight="8dp">

     <ListView android:id="@android:id/list"
               android:layout_width="match_parent"
               android:layout_height="match_parent"
               android:background="#00FF00"
               android:layout_weight="1"
               android:drawSelectorOnTop="false"/>

     <TextView android:id="@android:id/empty"
               android:layout_width="match_parent"
               android:layout_height="match_parent"
               android:background="#FF0000"
               android:text="No data"/>
 </LinearLayout>
```

##### Row Layout(行布局)

You can specify the layout of individual rows in the list. You do this by specifying a layout resource in the ListAdapter object hosted by the activity (the ListAdapter binds the ListView to the data; more on this later).

A ListAdapter constructor takes a parameter that specifies a layout resource for each row. It also has two additional parameters that let you specify which data field to associate with which object in the row layout resource. These two parameters are typically parallel arrays.

Android provides some standard row layout resources. These are in the R.layout class, and have names such as simple_list_item_1, simple_list_item_2, and two_line_list_item. The following layout XML is the source for the resource two_line_list_item, which displays two data fields,one above the other, for each list row.

```
 <?xml version="1.0" encoding="utf-8"?>
 <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     android:orientation="vertical">

     <TextView android:id="@+id/text1"
         android:textSize="16sp"
         android:textStyle="bold"
         android:layout_width="match_parent"
         android:layout_height="wrap_content"/>

     <TextView android:id="@+id/text2"
         android:textSize="16sp"
         android:layout_width="match_parent"
         android:layout_height="wrap_content"/>
 </LinearLayout>
```

You must identify the data bound to each TextView object in this layout. The syntax for this is discussed in the next section.

##### Binding to Data

You bind the ListActivity's ListView object to data using a class that implements the ListAdapter interface. Android provides two standard list adapters: SimpleAdapter for static data (Maps), and SimpleCursorAdapter for Cursor query results.

The following code from a custom ListActivity demonstrates querying the Contacts provider for all contacts, then binding the Name and Company fields to a two line row layout in the activity's ListView.

```
public class MyListAdapter extends ListActivity {

     @Override
     protected void onCreate(Bundle savedInstanceState){
         super.onCreate(savedInstanceState);

         // We'll define a custom screen layout here (the one shown above), but
         // typically, you could just use the standard ListActivity layout.
         setContentView(R.layout.custom_list_activity_view);

         // Query for all people contacts using the Contacts.People convenience class.
         // Put a managed wrapper around the retrieved cursor so we don't have to worry about
         // requerying or closing it as the activity changes state.
         mCursor = this.getContentResolver().query(People.CONTENT_URI, null, null, null, null);
         startManagingCursor(mCursor);

         // Now create a new list adapter bound to the cursor.
         // SimpleListAdapter is designed for binding to a Cursor.
         ListAdapter adapter = new SimpleCursorAdapter(
                 this, // Context.
                 android.R.layout.two_line_list_item,  // Specify the row template to use (here, two columns bound to the two retrieved cursor
 rows).
                 mCursor,                                              // Pass in the cursor to bind to.
                 new String[] {People.NAME, People.COMPANY},           // Array of cursor columns to bind to.
                 new int[] {android.R.id.text1, android.R.id.text2});  // Parallel array of which template objects to bind to those columns.

         // Bind to our new adapter.
         setListAdapter(adapter);
     }
 }
```

一种Activity，通过绑定到数据源（如数组或光标）来显示项目列表，并在用户选择项目时公开事件处理程序。

ListActivity托管一个可以绑定到不同数据源的ListView对象，通常是一个数组或一个Cursor持有查询结果。以下各节将讨论绑定，屏幕布局和行布局。

##### 屏幕布局

ListActivity的默认布局由屏幕中央的单个全屏列表组成。但是，如果您愿意，可以通过在onCreate（）中使用setContentView（）设置您自己的视图布局来自定义屏幕布局。要做到这一点，你自己的视图必须包含一个ID为“@android：id / list”的ListView对象（或者如果它在代码中的话）

或者，您的自定义视图可以包含任何类型的另一个视图对象，以便在列表视图为空时显示。这个“空列表”通知器必须有一个ID“android：id / empty”。请注意，当存在空白视图时，当没有要显示的数据时，列表视图将被隐藏。

以下代码演示了一个（丑陋的）自定义屏幕布局。它有一个带有绿色背景的列表，以及另一个红色的“无数据”消息。

##### ##### 行布局

您可以在列表中指定单个行的布局。您可以通过在由活动托管的ListAdapter对象中指定布局资源（ListAdapter将ListView绑定到数据;稍后再详细讨论）来完成此操作。ListAdapter构造函数接受一个参数，该参数为每一行指定布局资源。它还有两个额外的参数，可让您指定哪个数据字段与行布局资源中的哪个对象相关联。这两个参数通常是并行阵列。

Android提供了一些标准的行布局资源。它们位于R.layout类中，并具有诸如simple_list_item_1，simple_list_item_2和two_line_list_item之类的名称。以下布局XML是资源two_line_list_item的源代码，它为每个列表行显示两个数据字段，一个在另一个之上。

您必须识别绑定到此布局中每个TextView对象的数据。这个语法在下一节讨论。

##### 绑定到数据

使用实现ListAdapter接口的类将ListActivity的ListView对象绑定到数据。Android提供了两个标准列表适配器：用于静态数据的SimpleAdapter（地图）和用于光标查询结果的SimpleCursorAdapter。

以下来自自定义ListActivity的代码演示了查询联系人提供程序的所有联系人，然后将Name和Company字段绑定到活动的ListView中的两行行布局。

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

