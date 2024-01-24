# Accessibility announcement - .NET MAUI

.NET MAUI has built-in support for posting an accessibility announcement to the VoiceOver or TalkBack engine.

```csharp
SemanticScreenReader.Default.Announce("Appt announcement");
```

If you want to use attributed strings or add a delay, you can create your own service, for example `IA11yService`.

```csharp
  public MainPage(IA11yService a11yService)
	{
		this.a11yService = a11yService;
		InitializeComponent();
  }


    protected override void OnAppearing()
    {
        base.OnAppearing();
        a11yService.Speak("Deze tekst is in het nederlands geschreven.", 250, "nl-NL");
    }

```

```csharp
// IA11YService for Android and iOS
public interface IA11YService
{
  bool IsInVoiceOverMode { get; }
  Task Speak(string? text, int pauseInMs = 0, string? language = null);
}
```

```csharp
// IA11YService implementation on iOS
public bool IsInVoiceOverMode => UIAccessibility.IsVoiceOverRunning;

public Task Speak(string? text, int pauseInMs = 0, string? language = null))
{
  if (IsInVoiceOverMode && !string.IsNullOrEmpty(text))
  {
    var dict = new NSMutableDictionary();

    if (language != null)
        dict.Add(UIView.SpeechAttributeLanguage, new NSString(language));

    await Task.Delay(pauseInMs);

    if (IsInVoiceOverMode && !string.IsNullOrEmpty(text))
    {
        if (pauseInMs > 0)
            dict.Add(UIView.SpeechAttributeQueueAnnouncement, new NSString("Yes"));

        UIAccessibility.PostNotification(UIAccessibilityPostNotification.Announcement, new NSAttributedString(str: text, attributes: dict));
    }
  }
}
```

```csharp
// IA11YService implementation on Android
private static AccessibilityManager? AccessibilityManager => Android.App.Application.Context.GetSystemService(Android.Content.Context.AccessibilityService) as AccessibilityManager;

public bool IsInVoiceOverMode => AccessibilityManager is { IsEnabled: true, IsTouchExplorationEnabled: true };

public async Task Speak(string? text, int pauseInMs = 0, string? language = null))
{
  if (IsInVoiceOverMode && !string.IsNullOrEmpty(text))
  {
    if (pauseInMs > 0)
      await Task.Delay(pauseInMs);
    try
    {
        if (language is null)
            SemanticScreenReader.Announce(text);
        else
            Announce(text, language);
    }
    catch (Exception e)
    {
      // Sometimes we get an exception: MauiContext must be set on parent
      System.Diagnostics.Debug.WriteLine($"A11YService.Android.Speak exception: {e.Message}");
    }
  }
}

private void Announce(string text, string language)
{
    var announcement = OperatingSystem.IsAndroidVersionAtLeast(33)
            ? new AccessibilityEvent()
#pragma warning disable 618
            : AccessibilityEvent.Obtain();
#pragma warning restore 618

    if (AccessibilityManager == null || announcement == null)
        return;

    if (!(AccessibilityManager.IsEnabled || AccessibilityManager.IsTouchExplorationEnabled))
        return;

    var locale = new Android.Text.Style.LocaleSpan(Java.Util.Locale.ForLanguageTag(language));
    var message = new Android.Text.SpannableString(text);
    message.SetSpan(locale, 0, message.Length(), Android.Text.SpanTypes.InclusiveInclusive);

    announcement.EventType = EventTypes.Announcement;
    announcement.Text?.Add(message);
    AccessibilityManager.SendAccessibilityEvent(announcement);

}
```
