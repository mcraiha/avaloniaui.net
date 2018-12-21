Title: Creating the Project
Order: 10
---

## Visual Studio

The easiest way to get started with Avalonia from Visual Studio is to [install the extension]
(https://marketplace.visualstudio.com/items?itemName=AvaloniaTeam.AvaloniaforVisualStudio) from the
Visual Studio Marketplace.

Once that is installed, you can create an Avalonia MVVM application:

![New Project Dialog](images/new-project-dialog.png)

- Start Visual Studio
- Select File -> New -> Project
- In the Avalonia section, select "Avalonia MVVM Application"
- Enter "Todo" as the Name
- Click OK

> Note: ignore the "Framework" selection - it doesn't have any effect on this template.

## .NET Core

First install the Avalonia templates for .NET Core by following the instructions
[here](https://github.com/AvaloniaUI/avalonia-dotnet-templates).

Now you can create the application from the template:

```powershell
dotnet new avalonia.app.mvvm -o Todo -n Todo
```

## Project structure

The newly created project will be pre-filled with a number of files and directories:

```
Todo
 |- Assets
 |   |- avalonia-logo.ico
 |- Models 
 |- ViewModels
 |   |- MainWindowViewModel.cs
 |   |- ViewModelBase.cs
 |- Views
 |   |- MainWindow.xaml
 |- App.xaml
 |- Program.cs
 |- ViewLocator.cs
```

You can see there are directories for each of the concepts in the MVVM pattern (models, views and view models) as well as couple of other files and directories:

- The **Assets** directory holds the binary assets for your application such as icons and bitmaps. Files placed in this directory will automatically be included as resources in the application.
- The **Models** directory is currently empty, but as the name suggests this is where we'll be putting our models.
- The **ViewModels** directory is pre-filled with a base class for view models and a view model for the application main window.
- The **Views** directory just contains the application main window for now.
- The **App.xaml** file is where XAML styles and templates that will apply to the entire application will be placed.
- The **Program.cs** file is the entry point for execution of the application. Here you can configure the platform options for Avalonia if necessary.
- The **ViewLocator.cs** file is used to look up views for view models. This will be explained in more detail later.
