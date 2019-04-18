Title: Routing
Order: 0
---
[ReactiveUI routing](https://reactiveui.net/docs/handbook/routing/) consists of an [IScreen](https://reactiveui.net/api/reactiveui/iscreen/) that contains current [RoutingState](https://reactiveui.net/api/reactiveui/routingstate/), several [IRoutableViewModel](https://reactiveui.net/api/reactiveui/iroutableviewmodel/)s, and a platform-specific XAML control called [RoutedViewHost](https://github.com/AvaloniaUI/Avalonia/blob/55458cf7af24d6c987268ab5ff8a1ead1173310b/src/Avalonia.ReactiveUI/RoutedViewHost.cs). `RoutingState` manages the view model navigation stack and allows view models to navigate to other view models. `IScreen` is the root of a navigation stack; despite the name, its views don't have to occupy the whole screen. `RoutedViewHost` monitors an instance of `RoutingState`, responding to any changes in the navigation stack by creating and embedding the appropriate view.

TODO
- copy-paste view model examples from rxui docs?
- write view classes using Avalonia routed host from scratch