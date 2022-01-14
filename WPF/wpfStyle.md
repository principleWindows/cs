# Style

[WPF样式（Style）入门](https://blog.csdn.net/qq_34802416/article/details/78231575)

****************************

样式（Style）又是前端代码中非常重要的元素。

## 1 A simple example

先看一个最直观的例子（参见 gradeAid.Label2_S1.xaml）

```xml
<base:childPage x:Class="gradeAid.businessLogical.Label2_S1"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
      xmlns:local="clr-namespace:gradeAid.businessLogical" 
      xmlns:sys="clr-namespace:System;assembly=mscorlib" 
      xmlns:base="clr-namespace:gradeAid.UI" 
      mc:Ignorable="d" 
      d:DesignHeight="300" d:DesignWidth="1024" 
      Title="期末成绩" Loaded="ChildPage_Loaded">
    
    <Page.Resources>
        <FontFamily x:Key="ButtonFontFamily">Times New Roman</FontFamily>
        <sys:Double x:Key="ButtonFontSize">18</sys:Double>
        <FontWeight x:Key="ButtonFontWeight">Bold</FontWeight>
    </Page.Resources>

……

        <Button x:Name="cmd" Content="Resource Button" 
                Grid.Row="0" Grid.Column="1"
                Width="150" Height="30" Margin="3"
                FontFamily="{StaticResource ButtonFontFamily}"
                FontSize="{StaticResource ButtonFontSize}"
                FontWeight="{StaticResource ButtonFontWeight}"/>

……

</base>
```

【**解释**】
- 上面给当前的 Page 添加了三个静态资源：一个是 FontFamily 对象，定义了字体名称；
另一个是存储数字 18 的 double 对象定义字体的大小（需要引用
xmlns:sys="clr-namespace:System;assembly=mscorlib" 命名空间）；
第三个是枚举值 FontWeightBold。（使用资源最常见的原因之一便是通过它们保存样式）
- 在元素中直接使用这些资源，因为在应用程序的整个生命周期中，这些资源都不会发生变化，
所以使用静态资源是合理的。
- 最后就得了一个应用了样式的按钮。
- **注**：样式设置元素的初始外观，但可以随意覆盖它们设置的这些特性。如再在 Button
元素中明确设置 Fontsize=”20”那么按钮标签中的 FontSize 设置会被覆盖。


## 2 Simplifying the simple example

上面例子中的方法有点冗杂，这里稍加改进（参见 gradeAid.Label0_S2.xaml）。

```xml
<base:childPage x:Class="gradeAid.businessLogical.Label0_S2"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
      xmlns:local="clr-namespace:gradeAid.businessLogical" 
      xmlns:sys="clr-namespace:System;assembly=mscorlib" 
      xmlns:base="clr-namespace:gradeAid.UI" 
      mc:Ignorable="d" 
      d:DesignHeight="300" d:DesignWidth="1024" 
      Title="Label0_S2" Loaded="ChildPage_Loaded">
    
    <Page.Resources>
        <Style x:Key="BigFontButtonStyle">
            <Setter Property="Control.FontFamily" Value="Times New Roman"/>
            <Setter Property="Control.FontSize" Value="18"/>
            <Setter Property="Control.FontWeight" Value="Bold"/>
        </Style>
    </Page.Resources>

……

            <Button x:Name="cmd" Content="Resource Button" 
                    Grid.Row="5" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource BigFontButtonStyle}"/>

……

</base>
```

要使用上面的样式资源，还需要在后台代码中进行样式设置：
```csharp
            cmd.Style = (Style)cmd.FindResource("BigFontButtonStyle");
```

【**解释**】
- 上面给当前的 Page 创建了一个独立资源：一个 System.Page.Style 对象。
该资源对象包含三个 Setter 对象，每个 Setter 对象对应一个可以设置的属性。
该资源对象的 Key 名用于引用该样式。
- Setter 对象中的 Property 设置是针对 Control 类型，而不仅仅是 Button
类型。它们对其他包含 FontFamily、FontSize、FontWeight 
的控件都有效。当然可以使用 TargetType 属性限定该样式可以引用的对象，例如：
```xml
        <Style x:Key="BigFontButtonStyle" TargetType="Button">
```
这样就只能允许 Button 控件引用该样式。


## 3 Trigger

WPF的样式是非常强大的，除了与HTML标记中的CSS类似，它还能够支持触发器（Trigger）。
比如当元素属性发生变化时，可通过触发器改变控件样式。
实现方法可以通过创建 Style 的 EventSetter 对象的集合
（参见 gradeAid.Label0_S3.xaml）。

```xml
<base:childPage x:Class="gradeAid.businessLogical.Label0_S3"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
      xmlns:local="clr-namespace:gradeAid.businessLogical" 
      xmlns:sys="clr-namespace:System;assembly=mscorlib" 
      xmlns:base="clr-namespace:gradeAid.UI" 
      mc:Ignorable="d" 
      d:DesignHeight="300" d:DesignWidth="1024" 
      Title="Label0_S3" Loaded="ChildPage_Loaded">
    
    <Page.Resources>
        <Style x:Key="MouseOverHighlightStyle" TargetType="TextBlock">
            <Setter Property="HorizontalAlignment" Value="Center"/>
            <Setter Property="TextAlignment" Value="Center"/>
            <Setter Property="Padding" Value="5"/>
            <EventSetter Event="TextBlock.MouseEnter" Handler="element_MouseEnter"/>
            <EventSetter Event="TextBlock.MouseLeave" Handler="element_MouseLeave"/>
        </Style>
    </Page.Resources>

……

            <TextBlock Text="This is a TextBlock" 
                    Grid.Row="7" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource MouseOverHighlightStyle}"/>

……

</base>
```

后台代码中的事件处理函数：
```csharp
private void element_MouseEnter(object sender,MouseEventArgs e)
{
    ((TextBlock)sender).Background = new SolidColorBrush(Colors.Aqua);
}

private void element_MouseLeave(object sender, MouseEventArgs e)
{
    ((TextBlock)sender).Background = null;
}
```

【**解释**】

- MouseEnter 和MouseLeave 事件完成了背景颜色改变。在 TextBlock 
标签中，只需要应用一行 Style=”{StaticResource MouseOverHighlightStyle}”
便可以实现，非常适合当需要为大量元素应用鼠标悬停效果时的情况。基于样式的事件处理程序轻松简单。
- 但 WPF 中极少使用事件设置器这种技术，更方便使用的是事件触发器。
它以声明的方式定义了所希望的行为（并且不需要任何代码）。


## 4 多层样式 —— 样式的继承

有时候我们希望在另一个样式的基础上创建样式，这时可通过为样式设置 BasedOn 
特性来使用此类样式继承，使用起来非常简单
（参见 gradeAid.Label0_S3.xaml）。

```xml
<base:childPage x:Class="gradeAid.businessLogical.Label0_S3"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
      xmlns:local="clr-namespace:gradeAid.businessLogical" 
      xmlns:sys="clr-namespace:System;assembly=mscorlib" 
      xmlns:base="clr-namespace:gradeAid.UI" 
      mc:Ignorable="d" 
      d:DesignHeight="300" d:DesignWidth="1024" 
      Title="Label0_S3" Loaded="ChildPage_Loaded">
    
    <Page.Resources>
        <Style x:Key="MouseOverHighlightStyle" TargetType="TextBlock">
            <Setter Property="HorizontalAlignment" Value="Center"/>
            <Setter Property="TextAlignment" Value="Center"/>
            <Setter Property="Padding" Value="5"/>
            <EventSetter Event="TextBlock.MouseEnter" Handler="element_MouseEnter"/>
            <EventSetter Event="TextBlock.MouseLeave" Handler="element_MouseLeave"/>
        </Style>

        <Style x:Key="BaseOnStyle" 
               TargetType="TextBlock" 
               BasedOn="{StaticResource MouseOverHighlightStyle}">
            <Setter Property="Control.Foreground" Value="Red"/>
        </Style>
    </Page.Resources>

……

            <TextBlock Text="This is a TextBlock" 
                    Grid.Row="7" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource MouseOverHighlightStyle}"/>

            <TextBlock Text="Inherited Style TextBlock" 
                    Grid.Row="8" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource BaseOnStyle}"/>

……

</base>
```

【**解释**】

- 第二个 Textbox 继承了第一个 Textbox 的样式，并在其基础上将文字颜色（Foreground）
修改为了 Red（红色）。


## 5 通过类型自动应用样式

当我们需要为界面的所有 Button 设置统一样式的时候怎么做？当 Button 比较少的时候，
我们可以用上面的方法逐个设置样式，但是当 Button 非常多的时候，这样的方法就显得麻烦了。
这个时候我们就可以使用 TargetType 来自动的为对应的控件应用样式。
例子如下（参见 gradeAid.Label0_S3.xaml）：

```xml
<base:childPage x:Class="gradeAid.businessLogical.Label0_S3"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
      xmlns:local="clr-namespace:gradeAid.businessLogical" 
      xmlns:sys="clr-namespace:System;assembly=mscorlib" 
      xmlns:base="clr-namespace:gradeAid.UI" 
      mc:Ignorable="d" 
      d:DesignHeight="300" d:DesignWidth="1024" 
      Title="Label0_S3" Loaded="ChildPage_Loaded">
    
    <Page.Resources>
        <Style x:Key="MouseOverHighlightStyle" TargetType="TextBlock">
            <Setter Property="HorizontalAlignment" Value="Center"/>
            <Setter Property="TextAlignment" Value="Center"/>
            <Setter Property="Padding" Value="5"/>
            <EventSetter Event="TextBlock.MouseEnter" Handler="element_MouseEnter"/>
            <EventSetter Event="TextBlock.MouseLeave" Handler="element_MouseLeave"/>
        </Style>

        <Style x:Key="BaseOnStyle" 
               TargetType="TextBlock" 
               BasedOn="{StaticResource MouseOverHighlightStyle}">
            <Setter Property="Control.Foreground" Value="Red"/>
        </Style>

        <Style TargetType="TextBlock" 
               BasedOn="{StaticResource MouseOverHighlightStyle}">
            <Setter Property="Control.Foreground" Value="Green"/>
        </Style>
    </Page.Resources>

……

            <TextBlock Text="This is a TextBlock" 
                    Grid.Row="7" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource MouseOverHighlightStyle}"/>

            <TextBlock Text="Inherited Style TextBlock" 
                    Grid.Row="8" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource BaseOnStyle}"/>

            <TextBlock Text="Global Style TextBlock" 
                    Grid.Row="9" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center"/>

            <TextBlock Text="Global Without Style TextBlock" 
                    Grid.Row="10" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{x:Null}"/>

……

</base>
```

【**解释**】

- 使用类型标记扩展来隐式的设置键名，样式会自动应用与整个元素树的所有 TextBlock 上
- 正如之前所说的，Style 可以被覆盖，前两个 TextBlock 
为自己提供了一个新样式；而第三个并没有，所以第三个自动的应用了该样式；第四个将 
Style 属性设置为 null 值，这样就有效的删除了样式

## 6 小结

通过上面几个例子对样式有一定的理解。样式触发器更难一些，并且具有更丰富的功能，将另文解读。

