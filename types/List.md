# List



## 2 ObservableCollection vs List

1. [Common mistakes while using ObservableCollection](https://updatecontrols.net/doc/tips/common_mistakes_observablecollection)
2. [How to: Create and Bind to an ObservableCollection](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/data/how-to-create-and-bind-to-an-observablecollection)
3. [ObservableCollection和List的区别总结](https://www.cnblogs.com/zyj649261718/p/8072679.html)

***************************

ObservableCollection is one of the most useful classes in WPF and 
Silverlight data binding. Whenever you modify the collection, the view 
is notified. Unfortunately, it is extremely easy to misuse. 

ObservableCollection 继承了：
- Collection：为泛型集合提供基类；
- INotifyCollectionChanged：将集合的动态更改通知给侦听器，例如，
何时添加和移除项或者重置整个集合对象；
- INotifyPropertyChanged：向客户端发出某一属性值已更改的通知。

ObservableCollection 的方法，对数据的操作很少，
重点放在了当自己本身变化的时候(不管是属性，还是集合)会 raise CollectionChanged / 
PropertyChanged 事件(可用于更新 UI)。

The advantage of ObservableCollection over List is that it implements 
INotifyCollectionChanged. This interface raises an event whenever the 
collection is modified by insertion, removal, or replacement. A List 
does not implement this interface, so anything data bound to it will 
not be notified when it changes.


### 2.1 Replacing the collection

When a view binds to an ObservableCollection, it subscribes to events from 
that instance. It expects to be notified when items are added, removed, or 
replaced within that instance of the collection. The binding is not to the 
property of the parent object, but to that specific instance of the collection 
itself.

A common mistake is to modify the parent object’s property and replace one 
ObservableCollection with another. Just about every person who uses 
ObservableCollections makes this mistake at least once. Even Scott Hanselman 
made this mistake on stage at Mix ‘09 (skip to 10:30). The problem is that 
after you swap out one ObservableCollection for a new one, the view is still 
bound to the old one.

A common “fix” for this problem is to fire a PropertyChanged event to notify 
the view that the collection has been replaced. This causes the view to unbind 
from the old one and rebind to the new one. In this situation, the view can’t 
adjust to just the items that were added or removed; it has to rebind the entire 
list box. Not only is this inefficient, but it also scrolls back to the top of 
the list rather than preserving the user’s context.

This fix completely obviates the need for an ObservableCollection in the first 
place. It wasn’t the collection that changed, it was the property that points 
to the collection. That property could have pointed to a List, and it would 
have had the same effect. Which brings us to the next common mistake.


### 2.2 Using ObservableCollection for static collections

A common misconception is that data binding requires ObservableCollection. 
This is not true. Data binding works against anything that implements 
IEnumerable. You can data bind a list box directly to a List, if you want.

The advantage of ObservableCollection over List is that it implements 
INotifyCollectionChanged. This interface raises an event whenever the 
collection is modified by insertion, removal, or replacement. A List does not 
implement this interface, so anything data bound to it will not be notified 
when it changes.

Not all collections change while the user is looking at them. Some collections 
are fixed. Others are loaded from a database or server once, and then simply 
displayed. If the collection doesn’t change, don’t make it an 
ObservableCollection. Just make it a List.


### 2.3 Using linq

ObservableCollection is imperative. Linq is declarative. The two cannot be 
used together without extra help.

Imperative code explicitly acts upon something. When using an 
ObservableCollection, you explicitly call Add, Remove, and other methods to 
change the collection. You have to decide exactly when and how to take these 
actions.

Declarative code implicitly generates something. When using linq, you declare 
what a collection will look like, how it will be filtered, mapped, and 
transformed. You don’t explicitly modify the collection.

These two paradigms don’t mix. Once you use linq on an ObservableCollection, 
it is no longer observable. There are open-source projects like Bindable Linq 
that bridge the gap. But without that extra help, people are often surprised 
when things don’t work.


### 2.4 Using it in the model

ObservableCollection is for data binding. In the MVVM pattern, you bind a 
view to a view model. So it stands to reason that ObservableCollection belongs 
in the view model. However, many people use it in the model as well.

Microsoft’s own code generators encourage us to make this mistake. When you 
add a reference to a WCF service returning a List, the proxy turns that into 
an ObservableCollection. When you generate classes from an Entity Framework 
model, you get ObservableCollections. Service calls and persistence belong in 
the model layer, not the view model layer. The view model needs to massage 
this data to make it suitable for the view, so it needs to map the model 
collection into its own ObservableCollection.


### 2.5 Modifying the collection on the background thread

When an ObservableCollection is modified, it raises an event. Events are 
handled synchronously. Event handlers respond on the same thread as the 
modification. The handler completes before the Add or Remove method returns. 
This means that you can’t modify an ObservableCollection in a background thread.

Most people who run into this problem realize their mistake quickly. They 
wrap the code that modifies the collection in a delegate, and invoke it on 
the UI thread. It is unfortunate that an ObservableCollection cannot be 
modified in a thread-safe way on a background thread. This fix puts more work 
than necessary on the UI thread, making it less responsive than it should be.


### 2.6 Conclusion

ObservableCollection is extremely easy to misuse. Even experts like Scott 
Hanselman get it wrong. Microsoft’s WCF and EF code generators lead us to 
incorrect patterns. And even when we try to do the right thing, the synchronous 
nature of CollectionChanged events get in our way.

My advice is to skip ObservableCollection altogether. Use Update Controls 
instead, and just [bind to a linq query](https://updatecontrols.net/doc/examples/linq.html). 
If you find that you need to access the collection programmatically, and 
not just through data binding, then [DependentList](https://updatecontrols.net/doc/advanced/fields-and-collections.html) 
might be right for you. 
Update Controls makes it much more difficult to make these common mistakes.





