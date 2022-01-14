# Coding Architecture of wpfTest


## 1 UI

`void MainWindow::createLeftMenuPage(string topMenuName)` 
负责调用各章的左侧菜单创建函数创建相应的左侧菜单。
用户点击主菜单 `MainMenuPage` 的相应章按钮时会调用来产生相应的左侧菜单。

### 1.1 Chapter 9

在 `MainMenuPage::label10_MouseLeftButtonDown_1(object sender, MouseButtonEventArgs e)` 中，
通过响应鼠标按键的点击产生第9章的左侧菜单。具体则通过调用
`Page MainWindow::createPageChapter9()` 来创建，负责管理该章的节操作内容选取。
该函数做了2件事情：
1. 创建第9章的节菜单 `MenuPageChapter9 page`，并将其返回给调用者将其设置为
`MainWindow::leftMenuFrame.Content = page;`
2. 将上面 `page` 的 `NextEvent` 加上对节菜单选取的事件响应的注册，例如设置框架的内容部分为
`contentFrame.Content = createPageChapter9_sy2();`

左侧菜单定义在 `MenuPageChapter9`中，在其 `excel_Click_1(object sender, RoutedEventArgs e)`
中激发 `FireNextEvent("chapter9_sy2")`，响应函数 `

在 IChildEvents 中声明了3个委托事件对象：

```csharp
    // 声明委托类型
    public delegate void ChildEventHandler(object arg);

    public  interface IChildEvents
    {

        // 声明委托事件的对象
        event ChildEventHandler NextEvent;

        event ChildEventHandler QuitEvent;

        event ChildEventHandler WaitIconEvent;
    }
```




