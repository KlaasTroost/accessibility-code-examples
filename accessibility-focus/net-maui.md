# Accessibility focus - .NET MAUI

.NET MAUI does have built-in support for changing accessibility focus.

The [`SetSemanticFocus()`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.semanticextensions.setsemanticfocus?view=net-maui-8.0#microsoft-maui-semanticextensions-setsemanticfocus(microsoft-maui-iview)) is an extension function of any control which implements the [`Microsoft.Maui.IView`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.iview?view=net-maui-8.0)-interface.

```csharp 
Entry.SetSemanticFocus();
//or
Button.SetSemanticFocus();
// any view or control implementing the IView interface
```
