Title: Adding new Items - Part II
Order: 70
---

Now we have the "Add new item" view appearing we need to make it work. First of all we need to
enable/disable the button depending on whether the user has typed anything in the `Description`.

# Implement the OK command

In the last section we bound a `Button.Command` to a method on the view model, but if we want to be
able to control the enabled state of the button we need to bind to an
[`ICommand`](/docs/binding/binding-to-commands). Again we're going to take advantage of ReactiveUI
and use [`ReactiveCommand`](https://reactiveui.net/docs/handbook/commands/).

:::filename
ViewModels\AddItemViewModel.cs
:::
```csharp
using System.Reactive;
using ReactiveUI;
using Todo.Models;

namespace Todo.ViewModels
{
    class AddItemViewModel : ViewModelBase
    {
        string description;

        public AddItemViewModel()
        {
            var okEnabled = this.WhenAnyValue(
                x => x.Description,
                x => !string.IsNullOrWhiteSpace(x));

            Ok = ReactiveCommand.Create(
                () => new TodoItem { Description = Description }, 
                okEnabled);
        }

        public string Description
        {
            get => description;
            set => this.RaiseAndSetIfChanged(ref description, value);
        }

        public ReactiveCommand<Unit, TodoItem> Ok { get; }
    }
}
```

First we modify the `Description` property to raise
[change notifications](/docs/binding/change-notification). We [saw this pattern
before](/docs/tutorial/adding-new-items#swap-out-the-list-view-model) in the main window view
model. In this case though, we're implementing change notifications for `ReactiveUI` rather than
for Avalonia specifically:


```csharp
var okEnabled = this.WhenAnyValue(
    x => x.Description,
    x => !string.IsNullOrWhiteSpace(x));
```

Now that `Description` has change notifications enabled we can use
[`WhenAnyValue`](https://reactiveui.net/docs/handbook/when-any/) to convert the property into a
stream of values in the form of an
[`IObservable`](http://introtorx.com/Content/v1.0.10621.0/02_KeyTypes.html#KeyTypes).

This above code can be read as:

- For the initial value of `Description`, and for subsequent changes
- Select the inverse of the result of invoking `string.IsNullOrWhiteSpace()` with the value

This means that `okEnabled` represents a stream of `bool` values which will produce `true` when
`Description` is a non-empty string and `false` when it is an empty string. This is exactly how we
want the `OK` button to be enabled.

We then create a `ReactiveCommand` and assign it to the `Ok` property:

```csharp
Ok = ReactiveCommand.Create(
    () => new TodoItem { Description = Description }, 
    okEnabled);
```

The second parameter to `ReactiveCommand.Create` controls the enabled state of the command, and
so the observable we just created is passed there.

The first parameter is a lambda that is run when the command is executed. Here we simply create
an instance of our model `TodoItem` with the description entered by the user.

# Bind the OK button

We can now bind the OK button in the view to the `Ok` command we just created on the view model:

:::filename
Views/AddItemView.xaml
:::
```xml{9}
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d" d:DesignWidth="200" d:DesignHeight="300"
             x:Class="Todo.Views.AddItemView">
  <DockPanel>
    <Button DockPanel.Dock="Bottom">Cancel</Button>
    <Button DockPanel.Dock="Bottom" Command="{Binding Ok}">OK</Button>
    <TextBox AcceptsReturn="False"
             Text="{Binding Description}"
             Watermark="Enter your TODO"/>
  </DockPanel>
</UserControl>
```

