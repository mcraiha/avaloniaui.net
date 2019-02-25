Title: Creating the List View Models
Order: 30
---

The next thing we want to do is display a list of the items read from the database. For this we're going to first need a view model which represents an item in the list. We'll call this `TodoItemViewModel`.

`ViewModels\TodoItemViewModel.cs`:
```csharp
using ReactiveUI;
using Todo.Models;

namespace Todo.ViewModels
{
    public class TodoItemViewModel : ViewModelBase
    {
        string description;
        bool isChecked;

        public TodoItemViewModel()
        {
        }

        public TodoItemViewModel(TodoItem source)
        {
            description = source.Description;
            isChecked = source.IsChecked;
        }

        public string Description
        {
            get => description;
            set => this.RaiseAndSetIfChanged(ref description, value);
        }

        public bool IsChecked
        {
            get => isChecked;
            set => this.RaiseAndSetIfChanged(ref isChecked, value);
        }
    }
}
```

You can see that this view model has the same structure as the `TodoItem` model, and it simply takes a model and populates its properties with the values from the model.

There are two things to notice about the view model:

- It derives from `ViewModelBase`. This itself derives from `ReactiveObject` which implements the [`INotifyPropertyChanged`](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifypropertychanged?view=netstandard-2.0) interface. `INotifyPropertyChanged` is part of the .NET standard library and it exposes an event (called `PropertyChanged`) which is fired when a property is changed.
- The property setters call `RaiseAndSetIfChanged`. This method handles updating the backing field and calling `PropertyChanged` if the value has changed.

`INotifyPropertyChanged` is important to Avalonia because it is by listening to the event on this interface that the user interface knows that something in the view model has changed, and that the UI should be updated.

Next we're going to need a view model which represents the list which we will call `TodoListViewModel`:

`ViewModels\TodoListViewModel.cs`:
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
        }

        public ObservableCollection<TodoItemViewModel> Items { get; }
    }
}
```

The list view model takes a collection of item models and for each one creates an item view model. It then puts these into an [`ObservableCollection<T>`](https://docs.microsoft.com/en-us/dotnet/api/system.collections.objectmodel.observablecollection-1?view=netframework-4.7.2) which is exposed via a property.

> You may thinking at this point that having a model and a view model which are essentially the same sounds like a lot of hassle. At this point, that may be the case but as we build up the application you'll start to see the two concepts diverge.
