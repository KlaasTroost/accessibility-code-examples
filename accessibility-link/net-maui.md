# Accessibility link - .NET MAUI

In .NET MAUI, you need to follow four steps to create links:

The text displayed by [Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label) and Span instances can be turned into hyperlinks with the following approach:

Set the `TextColor` and `TextDecoration` properties of the [Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label) or [Span](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.span).
Add a [TapGestureRecognizer](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.tapgesturerecognizer) to the `GestureRecognizers` collection of the [Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label) or [Span](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.span), whose `Command` property binds to a [ICommand](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input.icommand), and whose CommandParameter property contains the URL to open.
Define the [ICommand](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input.icommand) that will be executed by the [TapGestureRecognizer](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.tapgesturerecognizer).
Write the code that will be executed by the [ICommand](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input.icommand).

```xml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Read more about " />
            <Span Text="Appt"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://appt.org" />
                </Span.GestureRecognizers>
            </Span>
        </FormattedString>
    </Label.FormattedText>
</Label>
```