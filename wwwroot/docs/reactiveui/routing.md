Title: Routing
Order: 0
---
[ReactiveUI routing](https://reactiveui.net/docs/handbook/routing/) consists of an [IScreen](https://reactiveui.net/api/reactiveui/iscreen/) that contains current [RoutingState](https://reactiveui.net/api/reactiveui/routingstate/), several [IRoutableViewModel](https://reactiveui.net/api/reactiveui/iroutableviewmodel/)s, and a platform-specific XAML control called [RoutedViewHost](https://github.com/AvaloniaUI/Avalonia/blob/55458cf7af24d6c987268ab5ff8a1ead1173310b/src/Avalonia.ReactiveUI/RoutedViewHost.cs). `RoutingState` manages the view model navigation stack and allows view models to navigate to other view models. `IScreen` is the root of a navigation stack; despite the name, its views don't have to occupy the whole screen. `RoutedViewHost` monitors an instance of `RoutingState`, responding to any changes in the navigation stack by creating and embedding the appropriate view.

# Routing Example

**FirstViewModel.cs**

First, we create routable view models and corresponding views. We derive routable view models from the `IRoutableViewModel` interface from `ReactiveUI` namespace, and from `ReactiveObject` as well. `ReactiveObject` is the base class for [view model classes](https://reactiveui.net/docs/handbook/view-models/), and it implements `INotifyPropertyChanged`.

```cs
public class FirstViewModel : ReactiveObject, IRoutableViewModel
{
    public IScreen HostScreen { get; }

    public string UrlPathSegment => "first";

    public FirstViewModel(IScreen screen) => HostScreen = screen;
}
```

**FirstView.xaml**

```xml
<UserControl
    xmlns="https://github.com/avaloniaui"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="clr-namespace:ExampleApp;assembly=ExampleApp">
  <TextBlock Text="Hi, I'm the first view!"/>
</UserControl>
```

**FirstView.xaml.cs**

If we need to handle view model activation and deactivation, then we add a call to WhenActivated to the view. Generally, a rule of thumb is to always add the call to WhenActivated in your views, see [Activation](/docs/reactiveui/activation) docs for more info.

```cs
public partial class FirstView : ReactiveUserControl<FirstViewModel>
{
    public FirstView()
    {
        this.WhenActivated(disposables => { });
        AvaloniaXamlLoader.Load(this);
    }
}
```

**MainViewModel.cs**

Then, create a view model implementing the `IScreen` interface. It contains current `RoutingState` that manages the navigation stack. `RoutingState` also contains helper commands allowing you to navigate back and forward.

```cs
public class MainViewModel : ReactiveObject, IScreen
{
    // The Router associated with this Screen.
    // Required by the IScreen interface.
    public RoutingState Router { get; }
        
    // The command that navigates a user to first view model.
    public ReactiveCommand<Unit, IRoutableViewModel> GoNext { get; }

    // The command that navigates a user back.
    public ReactiveCommand<Unit, Unit> GoBack { get; }

    public MainViewModel()
    {
        // Initialize the Router.
        Router = new RoutingState();

        // Router uses Splat.Locator to resolve views for
        // view models, so we need to register our views
        // using Locator.CurrentMutable.Register* methods.
        //
        // Instead of registering views manually, you 
        // can use custom IViewLocator implementation,
        // see "View Location" section for details.
        //
        Locator.CurrentMutable.Register(() => new FirstView(), typeof(IViewFor<FirstViewModel>));

        // Manage the routing state. Use the Router.Navigate.Execute
        // command to navigate to different view models. 
        //
        // Note, that the Navigate.Execute method accepts an instance 
        // of a view model, this allows you to pass parameters to 
        // your view models, or to reuse existing view models.
        //
        GoNext = ReactiveCommand.CreateFromObservable(
            () => Router.Navigate.Execute(new FirstViewModel(this))
        );

        // You can also ask the router to go back.
        GoBack = Router.NavigateBack;
    }
}
```

**MainView.xaml**

Now we need to place the `RoutedViewHost` XAML control to our main view. It will resolve and embedd appropriate views for the view models. Note, that you need to import `rxui` namespace for `RoutedViewHost` to work.

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:rxui="http://reactiveui.net" 
        x:Class="ExampleApp.MainView"
        x:TypeArguments="vm:MainViewModel"
        xmlns:vm="clr-namespace:ExampleApp"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <rxui:RoutedViewHost Grid.Row="0" Router="{Binding Router}"/>
        <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="15">
            <Button Content="Go to first" Command="{Binding GoNext}" />
            <Button Content="Go back" Command="{Binding GoFirst}" />
        </StackPanel>
    </Grid>
</Window>
```

**MainView.xaml.cs**

Here is the code-behind for main view declared above.

```cs
public partial class MainView : ReactiveWindow<MainViewModel>
{
    public MainView()
    {
        DataContext = new MainViewModel();
        this.WhenActivated(disposables => { });
        AvaloniaXamlLoader.Load(this);
    }
}
```

TODO
- ensure it compiles
- show the results