# XAML Islands

- [Host WinRT XAML controls in desktop apps (XAML Islands)](https://docs.microsoft.com/en-us/windows/apps/desktop/modernize/xaml-islands)
- [Debug or disable project code in XAML Designer](https://docs.microsoft.com/en-us/visualstudio/xaml-tools/debugging-or-disabling-project-code-in-xaml-designer?view=vs-2017#control-display-options)
- [What is XAML Hot Reload for WPF and UWP apps? (Visual Studio)](https://docs.microsoft.com/en-us/visualstudio/xaml-tools/xaml-hot-reload?view=vs-2022)
- [Host a standard WinRT XAML control in a WPF app using XAML Islands](https://docs.microsoft.com/en-us/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands)

***************************



### Not supported


- XAML Islands do not support hosting a ContentDialog that contains a control that 
accepts text input, such as a TextBox, RichEditBox, or AutoSuggestBox. If you do 
this, the input control will not properly respond to key presses. To achieve similar 
functionality using a XAML Island, we recommend that you host a 
[**Popup**](../UWP/Windows.UI.Xaml.Controls.Primitives/Popup.md) that contains 
the input control.

## Chapter 2 - Host a standard WinRT XAML control in a WPF app

This Chapter demonstrates two ways to use XAML Islands to host a standard WinRT XAML 
control (that is, a first-party WinRT XAML control provided by the Windows SDK) in a 
WPF app that targets .NET Core 3.1:
- It shows how to host a UWP InkCanvas and InkToolbar controls by using **wrapped controls** 
in the Windows Community Toolkit. These controls wrap the interface and functionality of 
a small set of useful WinRT XAML controls. You can add them directly to the design 
surface of your WPF or Windows Forms project and then use them like any other WPF or 
Windows Forms control in the designer.

- It also shows how to host a UWP CalendarView control by using the **WindowsXamlHost** 
control in the Windows Community Toolkit. Because only a small set of WinRT XAML controls 
are available as wrapped controls, you can use WindowsXamlHost to host any other standard 
WinRT XAML control.


### 2.1 Required components

To host a WinRT XAML control in a WPF app, you'll need the following components in your 
solution. This article provides instructions for creating each of these components.

- The project and source code for your app. Using the **WindowsXamlHost** control to host 
WinRT XAML controls is currently supported only in apps that target .NET Core 3.x.

- A UWP app project that defines a root Application class that derives from XamlApplication. 
Your WPF project must have access to an instance of the 
**Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication** class provided by the Windows 
Community Toolkit so that it can discover and load custom UWP XAML controls. The recommended 
way to do this is to define this object in a separate UWP app project that is part of the 
solution for your WPF app.

> **Note** \
Although the XamlApplication object isn't required for hosting a first-party WinRT XAML 
control, your app needs this object to support the full range of XAML Island scenarios, 
including hosting custom WinRT XAML controls. Therefore, you are recommended that you 
always define a XamlApplication object in any solution in which you are using XAML Islands.

> **Note** \
Your solution can contain only one project that defines a XamlApplication object. The 
project that defines the XamlApplication object must include references to all other 
libraries and projects that are used to host WinRT XAML controls on the XAML Island.


### 2.2 Create a WPF project

Before getting started, follow these instructions to create a WPF project and configure 
it to host XAML Islands. If you have an existing WPF project, you can adapt these steps 
and code examples for your project.

1. In Visual Studio 2022, create a new WPF App (.NET Core) project. You must first install 
the latest version of the .NET Core 3.1 SDK if you haven't done so already.

2. Make sure package references are enabled:
    a. In Visual Studio, click Tools -> NuGet Package Manager -> Package Manager Settings.
    b. Make sure PackageReference is selected for Default package management format.

3. Right-click your WPF project in Solution Explorer and choose Manage NuGet Packages.

4. Select the Browse tab, search for the Microsoft.Toolkit.Wpf.UI.Controls package and 
install the latest stable version. This package provides everything you need to use the 
wrapped WinRT XAML controls for WPF (including InkCanvas and InkToolbar and the 
WindowsXamlHost control).

5. Configure your solution to target a specific platform such as x64. Most XAML Islands 
scenarios are not supported in projects that target **Any CPU**.
    a. In Solution Explorer, right-click the solution node and select Properties -> \
    Configuration Properties -> Configuration Manager.
    b. Under Active solution platform, select New.
    c. In the New Solution Platform dialog, select x64 and press OK.
    d. Close the open dialog boxes.


### 2.3 Define a XamlApplication class in a UWP app project

Next, add a UWP app project to your solution and revise the default App class in this 
project to derive from the Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication class 
provided by the Windows Community Toolkit. This class supports the IXamlMetadataProvider 
interface, which enables your app to discover and load metadata for custom UWP XAML 
controls in assemblies in the current directory of your application at run time. This 
class also initializes the UWP XAML framework for the current thread.

> **Note** \
Although this step isn't required for hosting a first-party WinRT XAML control, your app 
needs the XamlApplication object to support the full range of XAML Island scenarios, 
including hosting custom WinRT XAML controls. Therefore, we recommend that you always 
define a XamlApplication object in any solution in which you are using XAML Islands.

1. In Solution Explorer, right-click the solution node and select Add -> New Project.

2. Add a Blank App (Universal Windows) project to your solution. Make sure the target 
version and minimum version are both set to Windows 10, version 1903 (Build 18362) or a 
later release. Also make sure this new UWP project is not in a subfolder of the WPF 
project. Otherwise, the WPF app will later try to build the UWP XAML markup as if it 
were WPF XAML.

3. In the UWP app project, install the Microsoft.Toolkit.Win32.UI.XamlApplication 
NuGet package (latest stable version).

4. Open the App.xaml file and replace the contents of this file with the following XAML. 
Replace MyUWPApp with the namespace of your UWP app project.
    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Open the App.xaml.cs file and replace the contents of this file with the following 
code. Replace MyUWPApp with the namespace of your UWP app project.
    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Delete the MainPage.xaml file from the UWP app project.

7. Build the UWP app project.


### 2.4 Add a reference to the UWP project in your WPF project

1. Specify the compatible framework version in the WPF project file.

    a. In Solution Explorer, double-click the WPF project node to open the project file 
in the editor.

    b. In the first PropertyGroup element, add the following child element. Change the 19041 
portion of the value as necessary to match the target and minimum OS build of the UWP 
project.

    ```xml
    <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
    ```

2. In Solution Explorer, right-click the Dependencies node under the WPF project and add a 
reference to your UWP app project.


### 2.5 Instantiate the XamlApplication object in the entry point of your WPF app

Next, add code to the entry point for your WPF app to create an instance of the `App` 
class you just defined in the UWP project (this is the class that now derives from 
`XamlApplication`).

1. In your WPF project, right-click the project node, select Add -> New Item, and then 
select Class. Name the class **Program** and click **Add**.

2. Replace the generated `Program` class with the following code and then save the file. 
Replace `MyUWPApp` with the namespace of your UWP app project, and replace `MyWPFApp` 
with the namespace of your WPF app project.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Right-click the project node and choose Properties.

4. On the Application tab of the properties, click the **Startup object** drop-down and 
choose the fully qualified name of the `Program` class you added in the previous step.

> **Note** \
By default, WPF projects define a `Main` entry point function in a generated code file 
that isn't intended to be modified. This step changes the entry point for your project to 
the `Main` method of the new Program class, which enables you to add code that runs as 
early in the startup process of the app as possible.

5. Save your changes to the project properties.


### 2.6 Example: Host an InkCanvas and InkToolbar by using wrapped controls


### 2.7 Example: Host a CalendarView by using the host control


### 2.8 Package the app


