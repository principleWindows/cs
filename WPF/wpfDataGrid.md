# WPF DataGrid

- <https://blog.csdn.net/c0411034/article/details/82118904>
- [WPF中修改DataGrid单元格值并保存](https://www.cnblogs.com/yelanggu/p/10444632.html)
- [WPF的DataGrid中只能输入数字的列](https://blog.csdn.net/milijiangjun/article/details/82859014)
- [WPF DataGrid 中 直接添加/删除/编辑 数据](https://blog.csdn.net/Bruce_Admin/article/details/109910095)
- [C#datagrid与datagridview的区别](https://blog.csdn.net/weixin_43437202/article/details/88060563?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link)
- [Add new data sources](https://docs.microsoft.com/en-us/visualstudio/data-tools/add-new-data-sources?view=vs-2022)
- [Databinding with WPF](https://docs.microsoft.com/en-us/ef/ef6/fundamentals/databinding/wpf)

**************************

使用 DataGrid 你可以定义一个临时集合去装临时数据，然后把临时集合绑定在datagrid上。
DataGrid 绑定一个 400-500 行的数据并显示速度极快，根本感觉不到延迟卡顿。


## 1 数据显示

### 1.1 定义 XAML 文件


### 1.2 绑定数据


### 1.3 修改数据


### 1.4 修改某行某列的值和颜色（有BUG）


## 2 外观显示


### 2.1 让 DataGrid 中的列标题居中


### 2.2 让内容单元格居中显示


### 2.3 按百分比显示设置列的宽度


### 2.4 隔行变色


### 2.5 鼠标滑过行变色


### 2.6 选中行变色


### 2.7 取消单元格的边框


### 2.8 修改满足指定条件的行背景颜色


### 2.9 修改单元格的颜色


## 3 How do I dynamically generate columns in a WPF DataGrid?

<https://stackoverflow.com/questions/1983033/how-do-i-dynamically-generate-columns-in-a-wpf-datagrid>

*************************

I am attempting to display the results of a query in a WPF datagrid. The 
ItemsSource type I am binding to is `IEnumerable<dynamic>`. As the fields 
returned are not determined until runtime I don't know the type of the data 
until the query is evaluated. Each "row" is returned as an `ExpandoObject` 
with dynamic properties representing the fields.

It was my hope that `AutoGenerateColumns` (like below) would be able to 
generate columns from an `ExpandoObject` like it does with a static type but 
it does not appear to.

```xml
<DataGrid AutoGenerateColumns="True" ItemsSource="{Binding Results}"/>
```

Is there anyway to do this declaratively or do I have to hook in imperatively 
with some C#?

Ok this will get me the correct columns:

```csharp
// ExpandoObject implements IDictionary<string,object> 
IEnumerable<IDictionary<string, object>> rows = dataGrid1.ItemsSource.OfType<IDictionary<string, object>>();
IEnumerable<string> columns = rows.SelectMany(d => d.Keys).Distinct(StringComparer.OrdinalIgnoreCase);
foreach (string s in columns)
    dataGrid1.Columns.Add(new DataGridTextColumn { Header = s });
```

So now just need to figure out how to bind the columns to the IDictionary values.

Ultimately I needed to do two things:

1. Generate the columns manually from the list of properties returned by the query
2. Set up a DataBinding object

After that the built-in data binding kicked in and worked fine and didn't seem 
to have any issue getting the property values out of the `ExpandoObject`.

```xml
<DataGrid AutoGenerateColumns="False" ItemsSource="{Binding Results}" />
```

and

```csharp
// Since there is no guarantee that all the ExpandoObjects have the 
// same set of properties, get the complete list of distinct property names
// - this represents the list of columns
var rows = dataGrid1.ItemsSource.OfType<IDictionary<string, object>>();
var columns = rows.SelectMany(d => d.Keys).Distinct(StringComparer.OrdinalIgnoreCase);

foreach (string text in columns)
{
    // now set up a column and binding for each property
    var column = new DataGridTextColumn 
    {
        Header = text,
        Binding = new Binding(text)
    };

    dataGrid1.Columns.Add(column);
}
```





