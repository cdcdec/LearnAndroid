## ArrayAdapter

#### 继承体系

android.widget.ArrayAdapter<T>-->android.widget.BaseAdapter-->java.lang.Object 



You can use this adapter to provide views for an AdapterView, Returns a view for each object in a collection of data objects you provide, and can be used with list-based user interface widgets such as ListView or Spinner.

By default, the array adapter creates a view by calling toString() on each data object in the collection you provide, and places the result in a TextView. You may also customize what type of view is used for the data object in the collection. To customize what type of view is used for the data object, override getView(int, View, ViewGroup) and inflate a view resource. For a code example, see the CustomChoiceList sample.

For an example of using an array adapter with a ListView, see the Adapter Views guide.

For an example of using an array adapter with a Spinner, see the Spinners guide.

> Note: If you are considering using array adapter with a ListView, consider using RecyclerView instead. RecyclerView offers similar features with better performance and more flexibility than ListView provides. See the Recycler View guide.

您可以使用此适配器为AdapterView提供视图，为您提供的数据对象集合中的每个对象返回视图，并且可以与基于列表的用户界面小部件（如ListView或Spinner）一起使用。

默认情况下，数组适配器通过在您提供的集合中的每个数据对象上调用toString（）来创建一个视图，并将结果放入一个TextView中。您也可以自定义集合中用于数据对象的视图类型。要自定义用于数据对象的视图类型，请覆盖getView（int，View，ViewGroup）并为视图资源充气。有关代码示例，请参阅CustomChoiceList示例。

有关使用带有ListView的阵列适配器的示例，请参阅适配器视图指南。

有关在Spinner中使用阵列适配器的示例，请参阅Spinners指南。

> 如果您正在考虑在ListView中使用阵列适配器，请考虑使用RecyclerView.RecyclerView提供了类似的功能，具有比ListView提供的更好的性能和更大的灵活性。See the Recycler View guide.