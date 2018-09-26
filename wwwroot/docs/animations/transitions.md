Title: Transitions
Order: 10
---
Transitions in Avalonia are also heavily inspired by CSS Animations. They listen to any changes in target property's value 
and subsequently animates the change according to its parameters. They can be defined on any `Controls` via `Transitions` property:

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Styles>
        <Style Selector="Rectangle.red">
            <Setter Property="Height" Value="100"/>
            <Setter Property="Width" Value="100"/>
            <Setter Property="Background" Value="Red"/>
            <Setter Property="Opacity" Value="0.5"/> 
        </Style>
        <Style Selector="Rectangle.red:pointerover">
            <Setter Property="Opacity" Value="1"/> 
        </Style>
    </Window.Styles>

    <Rectangle Classes="red">
        <Rectangle.Transitions>
            <DoubleTransition Property="Opacity" Duration="0:0:0.2"/>
        </Rectangle.Transitions>
    </Rectangle>

<Window>
```

Transitions can also be defined in any style by using a `Setter` with `Transitions` as the target property and encapsulating them
in a `Transitions` object, like so:

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Styles>
        <Style Selector="Rectangle.red">
            <Setter Property="Height" Value="100"/>
            <Setter Property="Width" Value="100"/>
            <Setter Property="Background" Value="Red"/>
            <Setter Property="Opacity" Value="0.5"/> 
            <Setter Property="Transitions">
                <Transitions>
                    <DoubleTransition Property="Opacity" Duration="0:0:0.2"/>
                </Transitions>
            </Setter>
        </Style>
        <Style Selector="Rectangle.red:pointerover">
            <Setter Property="Opacity" Value="1"/> 
        </Style>
    </Window.Styles>

    <Rectangle Classes="red"/>

<Window>
```

Every transitions has a `Property`, `Delay`, `Duration` and an optional `Easing` property. 
Easing functions on [Animations](/doc/animations/animations#Easings) can be reused.

The following transition types are available:

* `DoubleTransitions`
* `FloatTransitions`
* `IntegerTransitions` 