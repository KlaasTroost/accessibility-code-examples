# Accessibility focusable - .NET MAUI

In .NET MAUI, the [`IsInAccessibleTree`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.automationproperties?view=net-maui-8.0) property indicates whether assistive technologies can focus on an element.

```xml
<Image AutomationProperties.IsInAccessibleTree="False" />
```
Image is not focusable for assistive technologies.

```xml 
<Image SemanticProperties.Description="Image description" />
```
Image is focusable for assistive technologies

```xml
<Image AutomationProperties.IsInAccessibleTree="False"
       SemanticProperties.Description="Image description" />
```
**Note**: `SemanticProperties.Description` will supersede the value of `AutomationProperties.IsInAccessibleTree`, so the Image is focusable for assistive technologies despite of the false value for 'IsInAccessibleTree' -->


