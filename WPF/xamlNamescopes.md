# WPF XAML Namescopes

<https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/wpf-xaml-namescopes>

[XAML中 x:Name和Name的区别](https://blog.csdn.net/weixin_46433786/article/details/109471781)

[AutomationProperties.Name VS x:Name](https://stackoverflow.com/questions/4605777/automationproperties-name-vs-xname)

[在WPF中，x：Name和Name属性之间有什么区别？](https://blog.csdn.net/p15097962069/article/details/103919768)

***********************************************************

XAML namescopes are a concept that identifies objects that are defined in XAML. The names 
in a XAML namescope can be used to establish relationships between the XAML-defined names 
of objects and their instance equivalents in an object tree. Typically, XAML namescopes in 
WPF managed code are created when loading the individual XAML page roots for a XAML 
application. XAML namescopes as the programming object are defined by the 
[INameScope](https://docs.microsoft.com/en-us/dotnet/api/system.windows.markup.inamescope) 
interface and are also implemented by the practical class 
[NameScope](https://docs.microsoft.com/en-us/dotnet/api/system.windows.namescope).


## 1 Namescopes in Loaded XAML Applications

In a broader programming or computer science context, programming concepts often include the 
principle of a unique identifier or name that can be used to access an object. For systems 
that use identifiers or names, the namescope defines the boundaries within which a process 
or technique will search if an object of that name is requested, or the boundaries wherein 
uniqueness of identifying names is enforced. These general principles are true for XAML 
namescopes. In WPF, XAML namescopes are created on the root element for a XAML page when the 
page is loaded. Each name specified within the XAML page starting at the page root is added 
to a pertinent XAML namescope.

In WPF XAML, elements that are common root elements (such as 
[Page](../System.Windows/Controls/pageClass.md), and 
[Window](../System.Windows/windowClass.md) always control a XAML namescope. If an element 
such as 
[FrameworkElement](../System.Windows/classFrameworkElement.md) 
or 
[FrameworkContentElement](../System.Windows/classFrameworkContentElement.md) 
is the root element of the page in markup, a XAML processor adds a Page root implicitly so that the Page can provide a working XAML namescope.

>  **Note**
>
> WPF build actions create a XAML namescope for a XAML production even if no `Name` or 
> `x:Name` attributes are defined on any elements in the XAML markup.

If you try to use the same name twice in any XAML namescope, an exception is raised. For 
WPF XAML that has code-behind and is part of a compiled application, the exception is raised 
at build time by WPF build actions, when creating the generated class for the page during the 
initial markup compile. For XAML that is not markup-compiled by any build action, exceptions 
related to XAML namescope issues might be raised when the XAML is loaded. XAML designers might 
also anticipate XAML namescope issues at design time.


## 2 Adding Objects to Runtime Object Trees


