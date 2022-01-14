# Style

[WPF��ʽ��Style������](https://blog.csdn.net/qq_34802416/article/details/78231575)

****************************

��ʽ��Style������ǰ�˴����зǳ���Ҫ��Ԫ�ء�

## 1 A simple example

�ȿ�һ����ֱ�۵����ӣ��μ� gradeAid.Label2_S1.xaml��

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
      Title="��ĩ�ɼ�" Loaded="ChildPage_Loaded">
    
    <Page.Resources>
        <FontFamily x:Key="ButtonFontFamily">Times New Roman</FontFamily>
        <sys:Double x:Key="ButtonFontSize">18</sys:Double>
        <FontWeight x:Key="ButtonFontWeight">Bold</FontWeight>
    </Page.Resources>

����

        <Button x:Name="cmd" Content="Resource Button" 
                Grid.Row="0" Grid.Column="1"
                Width="150" Height="30" Margin="3"
                FontFamily="{StaticResource ButtonFontFamily}"
                FontSize="{StaticResource ButtonFontSize}"
                FontWeight="{StaticResource ButtonFontWeight}"/>

����

</base>
```

��**����**��
- �������ǰ�� Page �����������̬��Դ��һ���� FontFamily ���󣬶������������ƣ�
��һ���Ǵ洢���� 18 �� double ����������Ĵ�С����Ҫ����
xmlns:sys="clr-namespace:System;assembly=mscorlib" �����ռ䣩��
��������ö��ֵ FontWeightBold����ʹ����Դ�����ԭ��֮һ����ͨ�����Ǳ�����ʽ��
- ��Ԫ����ֱ��ʹ����Щ��Դ����Ϊ��Ӧ�ó�����������������У���Щ��Դ�����ᷢ���仯��
����ʹ�þ�̬��Դ�Ǻ���ġ�
- ���͵���һ��Ӧ������ʽ�İ�ť��
- **ע**����ʽ����Ԫ�صĳ�ʼ��ۣ����������⸲���������õ���Щ���ԡ������� Button
Ԫ������ȷ���� Fontsize=��20����ô��ť��ǩ�е� FontSize ���ûᱻ���ǡ�


## 2 Simplifying the simple example

���������еķ����е����ӣ������ԼӸĽ����μ� gradeAid.Label0_S2.xaml����

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

����

            <Button x:Name="cmd" Content="Resource Button" 
                    Grid.Row="5" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource BigFontButtonStyle}"/>

����

</base>
```

Ҫʹ���������ʽ��Դ������Ҫ�ں�̨�����н�����ʽ���ã�
```csharp
            cmd.Style = (Style)cmd.FindResource("BigFontButtonStyle");
```

��**����**��
- �������ǰ�� Page ������һ��������Դ��һ�� System.Page.Style ����
����Դ����������� Setter ����ÿ�� Setter �����Ӧһ���������õ����ԡ�
����Դ����� Key ���������ø���ʽ��
- Setter �����е� Property ��������� Control ���ͣ����������� Button
���͡����Ƕ��������� FontFamily��FontSize��FontWeight 
�Ŀؼ�����Ч����Ȼ����ʹ�� TargetType �����޶�����ʽ�������õĶ������磺
```xml
        <Style x:Key="BigFontButtonStyle" TargetType="Button">
```
������ֻ������ Button �ؼ����ø���ʽ��


## 3 Trigger

WPF����ʽ�Ƿǳ�ǿ��ģ�������HTML����е�CSS���ƣ������ܹ�֧�ִ�������Trigger����
���統Ԫ�����Է����仯ʱ����ͨ���������ı�ؼ���ʽ��
ʵ�ַ�������ͨ������ Style �� EventSetter ����ļ���
���μ� gradeAid.Label0_S3.xaml����

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

����

            <TextBlock Text="This is a TextBlock" 
                    Grid.Row="7" Grid.Column="0"
                    Width="150" Height="70" Margin="3" 
                    HorizontalAlignment="Center" 
                    Style="{StaticResource MouseOverHighlightStyle}"/>

����

</base>
```

��̨�����е��¼���������
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

��**����**��

- MouseEnter ��MouseLeave �¼�����˱�����ɫ�ı䡣�� TextBlock 
��ǩ�У�ֻ��ҪӦ��һ�� Style=��{StaticResource MouseOverHighlightStyle}��
�����ʵ�֣��ǳ��ʺϵ���ҪΪ����Ԫ��Ӧ�������ͣЧ��ʱ�������������ʽ���¼�����������ɼ򵥡�
- �� WPF �м���ʹ���¼����������ּ�����������ʹ�õ����¼���������
���������ķ�ʽ��������ϣ������Ϊ�����Ҳ���Ҫ�κδ��룩��


## 4 �����ʽ ���� ��ʽ�ļ̳�

��ʱ������ϣ������һ����ʽ�Ļ����ϴ�����ʽ����ʱ��ͨ��Ϊ��ʽ���� BasedOn 
������ʹ�ô�����ʽ�̳У�ʹ�������ǳ���
���μ� gradeAid.Label0_S3.xaml����

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

����

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

����

</base>
```

��**����**��

- �ڶ��� Textbox �̳��˵�һ�� Textbox ����ʽ������������Ͻ�������ɫ��Foreground��
�޸�Ϊ�� Red����ɫ����


## 5 ͨ�������Զ�Ӧ����ʽ

��������ҪΪ��������� Button ����ͳһ��ʽ��ʱ����ô������ Button �Ƚ��ٵ�ʱ��
���ǿ���������ķ������������ʽ�����ǵ� Button �ǳ����ʱ�������ķ������Ե��鷳�ˡ�
���ʱ�����ǾͿ���ʹ�� TargetType ���Զ���Ϊ��Ӧ�Ŀؼ�Ӧ����ʽ��
�������£��μ� gradeAid.Label0_S3.xaml����

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

����

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

����

</base>
```

��**����**��

- ʹ�����ͱ����չ����ʽ�����ü�������ʽ���Զ�Ӧ��������Ԫ���������� TextBlock ��
- ����֮ǰ��˵�ģ�Style ���Ա����ǣ�ǰ���� TextBlock 
Ϊ�Լ��ṩ��һ������ʽ������������û�У����Ե������Զ���Ӧ���˸���ʽ�����ĸ��� 
Style ��������Ϊ null ֵ����������Ч��ɾ������ʽ

## 6 С��

ͨ�����漸�����Ӷ���ʽ��һ������⡣��ʽ����������һЩ�����Ҿ��и��ḻ�Ĺ��ܣ������Ľ����

