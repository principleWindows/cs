# Coding Architecture of wpfTest


## 1 UI

`void MainWindow::createLeftMenuPage(string topMenuName)` 
������ø��µ����˵���������������Ӧ�����˵���
�û�������˵� `MainMenuPage` ����Ӧ�°�ťʱ�������������Ӧ�����˵���

### 1.1 Chapter 9

�� `MainMenuPage::label10_MouseLeftButtonDown_1(object sender, MouseButtonEventArgs e)` �У�
ͨ����Ӧ��갴���ĵ��������9�µ����˵���������ͨ������
`Page MainWindow::createPageChapter9()` �����������������µĽڲ�������ѡȡ��
�ú�������2�����飺
1. ������9�µĽڲ˵� `MenuPageChapter9 page`�������䷵�ظ������߽�������Ϊ
`MainWindow::leftMenuFrame.Content = page;`
2. ������ `page` �� `NextEvent` ���϶Խڲ˵�ѡȡ���¼���Ӧ��ע�ᣬ�������ÿ�ܵ����ݲ���Ϊ
`contentFrame.Content = createPageChapter9_sy2();`

���˵������� `MenuPageChapter9`�У����� `excel_Click_1(object sender, RoutedEventArgs e)`
�м��� `FireNextEvent("chapter9_sy2")`����Ӧ���� `

�� IChildEvents ��������3��ί���¼�����

```csharp
    // ����ί������
    public delegate void ChildEventHandler(object arg);

    public  interface IChildEvents
    {

        // ����ί���¼��Ķ���
        event ChildEventHandler NextEvent;

        event ChildEventHandler QuitEvent;

        event ChildEventHandler WaitIconEvent;
    }
```




