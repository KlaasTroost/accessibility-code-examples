# Accessibility focus indicator - .NET MAUI

In .NET MAUI, you can adjust colors when an element receives focus. However, it's not possible to change the focus indicator of assistive technologies. Users can adjust their preferences in the system settings on Android and iOS.

The [`Visual State Manager`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.visualstatemanager?view=net-maui-8.0) (VSM) provides a structured way to make visual changes to the user interface from code. In this example the `VSM` contains a visual state group named `CommonState` which includes the `Focused` state. This indicator shows the focus by color and a border.

The code sample below shows how to change the background color of a button on focus.

```xml
    <Style
        TargetType="Button">
    </Style>
```
