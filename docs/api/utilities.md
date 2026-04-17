# API Reference — Utilities

## NfcHelper

**Path:** `util/NfcHelper.java`

Static utility for NFC tag processing.

### Methods

| Method | Description |
|--------|-------------|
| `getTagId(Tag tag)` | Extracts tag UID bytes, converts to uppercase hex string (e.g., `"025D8C5A754000"`) |

```java
public static String getTagId(Tag tag) {
    byte[] id = tag.getId();
    // Convert bytes to hex string
    return hexString;  // "025D8C5A754000"
}
```

---

## TtsHelper

**Path:** `util/TtsHelper.java`

Orchestrates text-to-speech with Edge TTS (primary) and Android TTS (fallback).

### Constructor

```java
TtsHelper(Context context, String language)  // "en" or "ar"
```

### Key Methods

| Method | Description |
|--------|-------------|
| `speak(String text)` | Speak text, cancel any previous speech |
| `speakThenRun(String text, Runnable onDone)` | Speak then execute callback when finished |
| `isSpeaking()` | Returns true if currently speaking |
| `stop()` | Stop all speech immediately |
| `shutdown()` | Release all resources |

### Generation Counter

Prevents overlapping speech when user taps rapidly:

```java
private volatile int speakGeneration = 0;

public void speak(String text) {
    stopAll();
    final int gen = ++speakGeneration;
    // ... callbacks check: if (gen != speakGeneration) return;
}
```

### Fallback Flow

```
speak("text")
  → Internet available?
    YES → edgeSpeak() → WebSocket → MP3 → MediaPlayer
      → Fails/Timeout(5s) → fallbackSpeakThenRun() → Android TTS
    NO  → fallbackSpeakThenRun() → Android TTS
```

---

## TashkeelHelper

**Path:** `util/TashkeelHelper.java`

Strips Arabic diacritical marks (tashkeel/harakat) for clean UI display.

### Method

```java
public static String strip(String text) {
    // Removes Unicode range [\u064B-\u0655\u0670]
    // Fathah, Dammah, Kasrah, Shaddah, Sukun, etc.
}
```

### Usage Pattern

- **Database:** Stores Arabic text WITH tashkeel (`حَلِيبُ المَرَاعِي`)
- **UI display:** `TashkeelHelper.strip()` → `حليب المراعي`
- **TTS speech:** Raw text with tashkeel → better pronunciation

---

## LocaleHelper

**Path:** `util/LocaleHelper.java`

Handles runtime language switching.

### Method

```java
public static Context setLocale(Context context, String language)
```

Applied in `App.attachBaseContext()` and `MainActivity.attachBaseContext()` to override the system locale.

---

## PreferencesManager

**Path:** `util/PreferencesManager.java`

Centralized SharedPreferences wrapper. All app settings in one place.

### Stored Preferences

| Key | Type | Default | Method |
|-----|------|---------|--------|
| `user_name` | String | "" | `getUserName()` / `setUserName()` |
| `dietary_profile` | String | "" | `getDietaryProfile()` / `setDietaryProfile()` |
| `tts_enabled` | boolean | true | `isTtsEnabled()` / `setTtsEnabled()` |
| `tts_speed` | int | 2 (x1) | `getTtsSpeed()` / `setTtsSpeed()` |
| `language` | String | "en" | `getLanguage()` / `setLanguage()` |
| `theme_mode` | String | "system" | `getThemeMode()` / `setThemeMode()` |
| `font_size` | String | "medium" | `getFontSize()` / `setFontSize()` |
| `ai_enabled` | boolean | true | `isAiEnabled()` / `setAiEnabled()` |
| `dark_mode` | boolean | false | `isDarkMode()` (legacy) |
| `large_text` | boolean | false | `isLargeText()` (legacy) |
| `first_launch` | boolean | true | `isFirstLaunch()` |
| `home_tutorial_shown` | boolean | false | `isHomeTutorialShown()` |
| `history_tutorial_shown` | boolean | false | `isHistoryTutorialShown()` |
| `settings_tutorial_shown` | boolean | false | `isSettingsTutorialShown()` |

### Computed Methods

| Method | Returns |
|--------|---------|
| `getEdgeTtsRate()` | "+0%", "+25%", "-30%" based on speed setting |
| `getAndroidTtsRate()` | 0.7f, 1.0f, 1.3f based on speed setting |
| `getFontScale()` | 0.85f, 1.0f, 1.3f based on font setting |
| `getNightMode()` | AppCompatDelegate constant for theme |
| `resetAll()` | Clears all preferences |
| `resetTutorials()` | Resets all tutorial flags |

---

## SpotlightOverlayView

**Path:** `util/SpotlightOverlayView.java`

Custom View that dims the screen and highlights UI elements with a spotlight cutout.

### Features

- Semi-transparent black overlay (80% alpha)
- Rounded rectangle or circle cutout per step
- Animated spotlight movement between steps (ValueAnimator)
- Tooltip card with title, description, step indicator, Next/Skip buttons
- Auto-positions tooltip above or below spotlight based on available space
- Auto-scrolls target into view (NestedScrollView/ScrollView support)
- Consumes all touch events (blocks navigation during tutorial)

### Key Methods

| Method | Description |
|--------|-------------|
| `addStep(View, String, String)` | Add rounded-rect spotlight step |
| `addCircleStep(View, String, String)` | Add circular spotlight step |
| `start()` | Begin tutorial sequence |
| `showNext()` | Advance to next step |

---

## TutorialManager

**Path:** `util/TutorialManager.java`

Static methods to show tutorials per screen.

| Method | Screen | Steps |
|--------|--------|-------|
| `showHomeTutorial()` | Home | NFC icon, search, recent scans, nav tabs (history, settings, about) |
| `showHistoryTutorial()` | History | Filter chips, clear button, history list |
| `showSettingsTutorial()` | Settings | Profile, dietary, accessibility, AI toggle, reset |
