# Accessibility focus indicator - .NET MAUI

In .NET MAUI, you can adjust colors when an element receives focus. However, it's not possible to change the focus indicator of assistive technologies. Users can adjust their preferences in the system settings on Android and iOS.

The [`Visual State Manager`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.visualstatemanager?view=net-maui-8.0) (VSM) provides a structured way to make visual changes to the user interface from code. In this example the `VSM` contains a visual state group named `CommonStates` which includes the `Focused` state. This indicator shows the focus by color and a border.

The code sample below shows how to change the background color of a button on focus.

NB. no special buttonhandler needed for Android, you can use the default ButtonHandler of MAUI.

```xml
    <Style TargetType="Button">
        <Setter Property="TextColor" Value="{AppThemeBinding Light={StaticResource White}, Dark={StaticResource Primary}}" />
        <Setter Property="Background" Value="{AppThemeBinding Light={StaticResource Primary}, Dark={StaticResource White}}" />
        <Setter Property="BorderColor" Value="{AppThemeBinding Light={StaticResource Primary}, Dark={StaticResource White}}" />
        <Setter Property="FontFamily" Value="OpenSansRegular"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="BorderWidth" Value="0"/>
        <Setter Property="CornerRadius" Value="8"/>
        <Setter Property="Padding" Value="14,10"/>
        <Setter Property="MinimumHeightRequest" Value="44"/>
        <Setter Property="MinimumWidthRequest" Value="44"/>
        <Setter Property="VisualStateManager.VisualStateGroups">
            <VisualStateGroupList>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="Background" Value="{AppThemeBinding Light={StaticResource Primary}, Dark={StaticResource White}}" />
                            <Setter Property="TextColor" Value="{AppThemeBinding Light={StaticResource White}, Dark={StaticResource Primary}}" />
                            <Setter Property="BorderColor" Value="{AppThemeBinding Light={StaticResource Primare}, Dark={StaticResource White}}" />
                            <Setter Property="BorderWidth" Value="4" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="Background" Value="{AppThemeBinding Light={StaticResource Gray300}, Dark={StaticResource White}}" />
                            <Setter Property="TextColor" Value="{AppThemeBinding Light={StaticResource White}, Dark={StaticResource Gray300}}" />
                            <Setter Property="BorderColor" Value="{AppThemeBinding Light={StaticResource Gray300}, Dark={StaticResource White}}" />
                            <Setter Property="BorderWidth" Value="4" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="PointerOver">
                        <VisualState.Setters>
                            <Setter Property="Background" Value="{AppThemeBinding Light={StaticResource Yellow300Accent}, Dark=Red}" />
                            <Setter Property="TextColor" Value="{AppThemeBinding Light=Red, Dark={StaticResource Yellow300Accent}}" />
                            <Setter Property="BorderColor" Value="{AppThemeBinding Light=Red, Dark={StaticResource Yellow300Accent}}" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="Background" Value="{AppThemeBinding Light={StaticResource White}, Dark={StaticResource Primary}}" />
                            <Setter Property="TextColor" Value="{AppThemeBinding Light={StaticResource Primary}, Dark={StaticResource White}}" />
                            <Setter Property="BorderColor" Value="{AppThemeBinding Light={StaticResource Primary}, Dark={StaticResource White}}" />
                            <Setter Property="BorderWidth" Value="4" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Background" Value="{AppThemeBinding Light={StaticResource Primary}, Dark={StaticResource White}}" />
                            <Setter Property="TextColor" Value="{AppThemeBinding Light={StaticResource White}, Dark={StaticResource Primary}}" />
                            <Setter Property="BorderColor" Value="{AppThemeBinding Light={StaticResource White}, Dark={StaticResource Primary}}" />
                            <Setter Property="BorderWidth" Value="4" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </Setter>
    </Style>
```

```csharp 
//File BorderedButtonHandler.iOS.cs
using UIKit;
using Microsoft.Maui.Handlers;

namespace A11YExamples.Handlers
{
    public partial class BorderedButtonHandler : ButtonHandler
    {
        protected override UIButton CreatePlatformView()
        {
            return new CustomBorderedButton(VirtualView);
        }
    }

    public class CustomBorderedButton : UIButton
    {
        private VisualElement? button;

        public CustomBorderedButton(IButton virtualView)
        {
            button = (VisualElement)virtualView; 
        }

        public override void DidUpdateFocus(UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            base.DidUpdateFocus(context, coordinator);

            if (button != null)
            {
                if (context.NextFocusedView == this)
                    VisualStateManager.GoToState(button, "Focused");
                else
                    VisualStateManager.GoToState(button, button.IsEnabled ? "Normal" : "Disabled");
            }
        }

        public override bool CanBecomeFocused => true;

    }
} 
```

To use the buttonhandler above you have code a line in you MauiProgram.cs

```csharp
// MauiProgram.cs
public static class MauiProgram
{
	public static MauiApp CreateMauiApp()
	{
		var builder = MauiApp.CreateBuilder();
		builder
			.UseMauiCommunityToolkit()
			.UseMauiApp<App>()
			.UseMauiCommunityToolkitCore()
            .UseMauiCommunityToolkitMediaElement()
            .ConfigureMauiHandlers(handlers =>
            {
 #if IOS
                handlers.AddHandler(typeof(Button), typeof(BorderedButtonHandler));
 #endif
            })
            .ConfigureFonts(fonts =>
			{
				fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
				fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
			});
		builder.Services.AddSingleton<IA11yService, A11yService>();
		builder.Services.AddTransient<AppShell>();
		builder.Services.AddTransient<MainPage>();


#if DEBUG
		builder.Logging.AddDebug();
#endif

        return builder.Build();
	}
}
```