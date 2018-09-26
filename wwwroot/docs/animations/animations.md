Title: Animations
Order: 0
---
Animations in Avalonia are heavily inspired by CSS Animations. They can be defined on any style using their
`Style.Animation` property:

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Styles>
        <Style Selector="Rectangle.red">
            <Setter Property="Height" Value="100"/>
            <Setter Property="Width" Value="100"/>
            <Setter Property="Background" Value="Red"/>
            <Style.Animations>
                <Animation Duration="0:0:1"> 
                    <KeyFrame Cue="0%">
                        <Setter Property="Opacity" Value="0.0"/>
                    </KeyFrame>
                    <KeyFrame Cue="100%">
                        <Setter Property="Opacity" Value="1.0"/>
                    </KeyFrame>
                </Animation>
            </Style.Animations>
        </Style>
    </Window.Styles>

    <Rectangle Classes="red"/>
<Window>
```

The example above animates the target `Control` as defined by its [selector](/docs/styles/selectors).
It will be run immediately when the control is loaded.

## Triggers

Unlike WPF's `Triggers`, Animations defined in XAML rely on [selectors](/docs/styles/selectors) for 
their triggering behavior. If the selector doesn't contain any pseudoclass, the animation will be
triggered when a matching `Control` is spawned into the visual tree. Otherwise, the animations will
run whenever its pseudoclass is activated. Also, when the selector turns negative (that means when a pseudoclass is 
no longer active or the control is removed), the currently running animation will therefore be canceled.

## `KeyFrames`

The `KeyFrame` objects defines when the target `Setter` objects should be applied on the target `Control`, 
with value interpolation in-between.

The `Cue` property of an `KeyFrame` object is based on the `Duration` of the parent animation and can be an 
absolute time index (i.e., `"0:0:1"`) or a percent of the animation's `Duration` (i.e., `"0%"`, `"100%"`).
However, `Cue`'s value should not exceed the `Duration` specified.

All `Animation` objects should contain at least one `KeyFrame`, with a `Setter` that has target property and value.

Multiple properties can be also animated in a single Animation by adding additional `Setter` objects on the 
desired `KeyFrame`:

```xml 
<Animation Duration="0:0:1"> 
    <KeyFrame Cue="0%">
        <Setter Property="Opacity" Value="0.0"/>
        <Setter Property="RotateTransform.Angle" Value="0.0"/>
    </KeyFrame>
    <KeyFrame Cue="100%">
        <Setter Property="Opacity" Value="1.0"/>
        <Setter Property="RotateTransform.Angle" Value="90.0"/>
    </KeyFrame>
</Animation>
```

## Delay

You can add a delay in a `Animation` by defining the desired delay time on its `Delay` property:

```xml
<Animation Duration="0:0:1"
           Delay="0:0:1"> 
    ...
</Animation>
```

## Repeat

You can set the following repeat behaviors on `RepeatCount` property of an `Animation`.

|Value|Description|
|-----|-----------|
|`None` or `0`|No Repeat|
|`1` to N|Repeat N times.|
|`Loop`|Repeat Indefinitely|

## Playback Direction

The `PlaybackDirection` property defines how should the animation be played, including repeats.

The following table describes the possible behaviors:

|Value|Description|
|-----|-----------|
|`Normal`|The animation is played normally.|
|`Reverse`|The animation is played in reverse direction.|
|`Alternate`|The animation is played forwards first, then backwards.|
|`AlternateReverse`|The animation is played backwards first, then forwards.|

## Value fill modes

The `FillMode` property defines whether the first or last interpolated value of an animation
persist before or after running an animation and on delays in-between runs. 

The following table describes the possible behaviors:

|Value|Description|
|-----|-----------|
|`None`|Value will not persist after animation nor the first value will be applied when the animation is delayed.|
|`Forward`|The last interpolated value will be persisted to the target property.|
|`Backward`|The first interpolated value will be displayed on animation delay.|
|`Both`|Both `Forward` and `Backward` behaviors will be applied.|

## Easings

Easing functions can be set by setting the name of the desired function to the `Animation`'s `Easing` property:

```xml
<Animation Duration="0:0:1"
           Delay="0:0:1"
           Easing="BounceEaseIn"> 
    ...
</Animation>
```

The following list contains the available easing functions. Adding your own easing functions are not yet
supported as of this document's writing.

* LinearEasing (Default)
* BackEaseIn
* BackEaseInOut
* BackEaseOut
* BounceEaseIn
* BounceEaseInOut
* BounceEaseOut
* CircularEaseIn
* CircularEaseInOut
* CircularEaseOut
* CubicEaseIn
* CubicEaseInOut
* CubicEaseOut
* ElasticEaseIn
* ElasticEaseInOut
* ElasticEaseOut
* ExponentialEaseIn
* ExponentialEaseInOut
* ExponentialEaseOut
* QuadraticEaseIn
* QuadraticEaseInOut
* QuadraticEaseOut
* QuarticEaseIn
* QuarticEaseInOut
* QuarticEaseOut
* QuinticEaseIn
* QuinticEaseInOut
* QuinticEaseOut
* SineEaseIn
* SineEaseInOut
* SineEaseOut