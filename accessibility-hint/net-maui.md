# Accessibility hint - .NET MAUI

In .NET MAUI you can set an accessibility hint by using the [`SemanticProperties.Hint`](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/accessibility?view=net-maui-8.0#hint) property.

```xml
<Button
  SemanticProperties.Hint="Opens the Appt website" />
```

```csharp
SemanticProperties.SetHint(button, "Opens the Appt website");
```
