Title: Textbox
---
The `TextBox` is editable text field where user can input text.

# Source code
[TextBox.cs](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Controls/TextBox.cs)

# Examples

## Basic one line TextBox

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaAppTemplate.MainWindow"
        Title="AvaloniaAppTemplate">
	<StackPanel Margin="10">
		<TextBox />
	</StackPanel>
</Window>
```
produces following output with **Windows 10**  
![Basic TextBox](images/textbox_basic.png)

## Password input TextBox

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaAppTemplate.MainWindow"
        Title="AvaloniaAppTemplate">
	<StackPanel Margin="10">
		<TextBox PasswordChar="#" />
	</StackPanel>
</Window>
```
produces following output with **Windows 10** when text is type to input  
![Password TextBox](images/textbox_password.png)

## TextBox with watermark

With Avalonia and WPF text suggestion that textbox shows is called watermark, but e.g. HTML5 calls it *placeholder*

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaAppTemplate.MainWindow"
        Title="AvaloniaAppTemplate">
	<StackPanel Margin="10">
		<TextBox Watermark="Street address" />
	</StackPanel>
</Window>
```
produces following output with **Windows 10**  
![Password TextBox](images/textbox_watermark.png)

## Multiline TextBox

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaAppTemplate.MainWindow"
        Title="AvaloniaAppTemplate">
	<Grid Margin="10">
		<TextBox AcceptsReturn="True" TextWrapping="Wrap" />
	</Grid>
</Window>
```
produces following output with **Windows 10** when text is type to input  
![Multiline TextBox](images/textbox_multiline.png)