Title: Adding new Items - Part 1
Order: 50
---

We now need a way to add new items. We'll start, as usual, by adding a view model. We're going to call this `EditTodoItemViewModel` as we'll later be using it to edit existing items too. We'll start of with a simple view model which just contains a `Description` property:

<div class="code-filename">ViewModels/EditTodoItemViewModel.cs</div>

```csharp
using ReactiveUI;

namespace Todo.ViewModels
{
    public class EditTodoItemViewModel : ViewModelBase
    {
        string description;

        public string Description
        {
            get => description;
            set => this.RaiseAndSetIfChanged(ref description, value);
        }
    }
}
```

The view will initially be pretty simple too: just a `TextBox` to allow input, with a watermark prompt and `AcceptsReturn="True"` which indicates a multi-line text box.

<div class="code-filename">Views/EditTodoItemView.xaml</div>

```xml
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="Todo.Views.EditTodoItemView">
    <TextBox Text="{Binding Description}"
             AcceptsReturn="True"
             Watermark="Enter your task here."/>
</UserControl>
```

Next we need a way to navigate to the "new item" view. Lets do this by adding a [command](docs/binding/binding-to-commands) called `NewItem` to the Todo list:

<div class="code-filename">ViewModels\TodoListViewModel.cs</div>

```csharp
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Linq;
using System.Reactive;
using ReactiveUI;
using Todo.Models;

namespace Todo.ViewModels
{
    public class TodoListViewModel : ViewModelBase
    {
        public TodoListViewModel(IEnumerable<TodoItem> items)
        {
            var viewModels = items.Select(x => new TodoItemViewModel(x));
            Items = new ObservableCollection<TodoItemViewModel>(viewModels);
            NewItem = ReactiveCommand.Create(() => { }); // <--- Command initialization
        }

        public ObservableCollection<TodoItemViewModel> Items { get; }
        public ReactiveCommand<Unit, Unit> NewItem { get; } // <--- Command declaration
    }
}
```

Here we're using ReactiveUI's [`ReactiveCommand`](https://reactiveui.net/docs/handbook/commands/) to create a command, and we assign that command to a public property.
You'll notice that we're passing an empty lambda to `ReactiveCommand.Create`, which means that when executed the command will not actually do anything. The reason for
this is that we're going to subscribe to the command from `MainWindowViewModel`:

<div class="code-filename">ViewModels/MainWindowViewModel.cs</div>

```csharp
using ReactiveUI;
using Todo.Services;

namespace Todo.ViewModels
{
    public class MainWindowViewModel : ViewModelBase
    {
        object content;

        public MainWindowViewModel()
        {
            var database = new Database();
            var items = database.GetItems();
            var list = new TodoListViewModel(items);
            list.NewItem.Subscribe(_ => NewItem()); // <--- Subscribe to the command
            content = list;
        }

        public object Content
        {
            get => content;
            private set => this.RaiseAndSetIfChanged(ref content, value);
        }

        void NewItem()
        {
            Content = new EditTodoItemViewModel();
        }
    }
}
```

We add a subscription to the `NewItem` command in the constructor, which calls the `NewItem` method. In the `NewItem` method we set the `Content` to a new instance of
`EditTodoItemViewModel`.

Finally we need to add a button to the list to invoke the command we just added:

<div class="code-filename">Views/TodoListView.xaml</div>

 ```xml
 <UserControl xmlns="https://github.com/avaloniaui"
              xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
              x:Class="Todo.Views.TodoListView">
    <!-- Add a containing DockPanel -->
    <DockPanel>
        <!-- Add a button and bind it to the `NewItem` command -->
        <Button DockPanel.Dock="Bottom" Command="{Binding NewItem}">
            Add New
        </Button>

        <ItemsControl Items="{Binding Items}">
            <ItemsControl.ItemTemplate>
                <DataTemplate>
                    <CheckBox IsChecked="{Binding IsChecked}" Content="{Binding Description}"/>
                </DataTemplate>
            </ItemsControl.ItemTemplate>
        </ItemsControl>
    </DockPanel>
</UserControl>
 ```

 First we place a `DockPanel` around the existing `ItemsControl`. A `UserControl` can contain only a single child, so if we want to have more than once children we need
 to use a `Panel`. `DockPanel` is a panel which will let us easily position the button at the bottom of the view by setting `DockPanel.Dock="Bottom"`. The button's 
 `Command` property is then bound to the `NewItem` command on the `TodoListViewModel` that we added earlier.

 If you now run the application and click the "Add New" button, you should now see the `EditTodoView` displayed!