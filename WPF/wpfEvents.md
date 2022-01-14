# Standard Object Lifetime Events

<http://www.myexceptions.net/program/1835775.html>

[#615 – Standard Object Lifetime Events for FrameworkElement Objects](https://wpf.2000things.com/2012/08/01/615-standard-object-lifetime-events-for-frameworkelement-objects/)

*********************

All objects in WPF that derive from FrameworkElement or FrameworkContentElement have three 
common events that fire as part of the element’s lifecycle.  Because these events are 
inherited from FrameworkElement and FrameworkContentElement, they are available for all 
controls, layout panels, Window objects and Page objects.

The three main object lifetime events are:

* **Initialized** – Fires when an object has been created and all of its properties have been 
set.  Properties relating to layout have not yet been set
* **Loaded** – Fires after the layout system has calculated all properties related to layout.  
Data binding has occurred at this point, so controls should have their final property values.
* **Unloaded** – Fires when element is removed from logical tree. Will not fire if application 
is shutting down.

功能：
- 增加按钮读取 excel 文档


