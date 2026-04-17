# Text-to-Speech

The app includes a powerful text-to-speech (TTS) system that reads product
information aloud. This feature is especially useful for visually impaired
users, hands-free shopping, or when you simply prefer to listen rather than
read.

---

## How Auto TTS Works

When **Auto TTS** is enabled in Settings, the app automatically reads product
information out loud immediately after a successful scan. The speech follows
a specific order:

### Standard Reading Order

1. **Product Name** — The name of the scanned product.
2. **Nutrition Summary** — Key nutritional values (calories, fat, protein, etc.).
3. **Ingredients** — The full ingredients list.
4. **Allergens** — Any allergens detected in the product.
5. **AI Recommendations** — Alternative product suggestions (if loaded).

### Allergen Priority

If the product contains allergens that **conflict with your dietary profile**,
the reading order changes:

1. **Allergen Warning** — Spoken first as a safety alert.
2. **Product Name**
3. **Nutrition Summary**
4. **Ingredients**
5. **AI Recommendations**

!!! warning
    When a conflicting allergen is detected, the app prioritizes speaking the
    allergen warning before anything else. This ensures you are alerted to
    potential dangers immediately.

---

## The Speak Aloud Button

A floating action button (FAB) is always visible on the product detail screen.
This button works **regardless of whether Auto TTS is enabled or disabled**.

- **Tap once** to start reading all product information from the beginning.
- **Tap while speech is active** to stop the current reading.

!!! tip
    Even if you have Auto TTS turned off, you can always tap the Speak Aloud
    FAB button to hear product details on demand. This gives you full control
    over when speech plays.

---

## Speech Engine

The app uses a two-tier speech system for the best possible voice quality:

### Primary: Edge TTS (Neural Voice)

- Uses Microsoft Edge neural text-to-speech for natural, human-like speech.
- Supports both **Arabic** and **English** voices.
- Produces clear, expressive pronunciation.
- Requires an internet connection.

### Fallback: Android TTS

- If Edge TTS fails to respond within **5 seconds**, the app automatically
  falls back to the built-in Android TTS engine.
- Android TTS works offline and does not require an internet connection.
- Voice quality is functional but less natural than Edge neural voices.

!!! note
    The 5-second timeout ensures you are never left waiting in silence. If your
    internet connection is slow or unavailable, the fallback activates quickly
    so you still hear the product information.

---

## Speech Speed Control

You can adjust how fast the voice speaks. Three speed options are available:

| Speed Setting | Description                                                      |
|---------------|------------------------------------------------------------------|
| **x0.5**      | Half speed. Slower and easier to follow for detailed information. |
| **x1.0**      | Normal speed. The default setting.                                |
| **x1.5**      | Faster speed. For experienced users who want quicker readouts.    |

Change the speech speed in **Settings > Speech Speed**.

!!! tip
    If you are new to the app or processing unfamiliar nutritional terms,
    start with x0.5 or x1.0. You can increase the speed once you are
    comfortable with the information flow.

---

## Allergen Pause Behavior

When the app reads allergens aloud, it inserts a **brief pause between each
allergen** for clarity. This is achieved using an ellipsis technique in the
speech text, which creates natural breathing pauses.

For example, instead of reading:

> "Contains milk, wheat, soy"

The app reads it as:

> "Contains... milk... wheat... soy"

This ensures each allergen is heard clearly and distinctly.

---

## AI Recommendations and TTS

AI recommendations are fetched from the internet and may take a few seconds
to load after the product details are already displayed.

- If Auto TTS finishes reading the product information **before recommendations
  load**, the recommendations are **spoken separately** once they arrive.
- You will hear a brief transition before the recommendations are read.
- If you have already stopped speech, the recommendations will not auto-play.
  You can tap the Speak Aloud FAB to hear them.

!!! note
    Recommendations are spoken separately because they depend on an internet
    response from the AI service. The app does not make you wait — it reads
    what is available immediately and adds recommendations when ready.

---

## Arabic Pronunciation and Tashkeel

For Arabic text-to-speech, the app uses **tashkeel** (diacritical marks) to
ensure correct pronunciation. Tashkeel marks indicate short vowels and
pronunciation details that are critical for accurate Arabic speech.

- Tashkeel is added to the text sent to the TTS engine for proper pronunciation.
- Tashkeel is **stripped from the on-screen text** so the display remains clean
  and readable.

This means what you hear is pronounced correctly, while what you see on screen
stays uncluttered.

---

## Controlling TTS Behavior

| Action                          | How to Do It                                     |
|---------------------------------|--------------------------------------------------|
| Enable/disable Auto TTS         | Settings > Auto TTS toggle                       |
| Change speech speed              | Settings > Speech Speed (x0.5, x1.0, x1.5)      |
| Read product info on demand      | Tap the Speak Aloud FAB button                   |
| Stop speech while playing        | Tap the Speak Aloud FAB button again              |

---

## Troubleshooting

| Problem                          | Solution                                          |
|----------------------------------|---------------------------------------------------|
| No sound after scan              | Check that Auto TTS is enabled in Settings. Check phone volume. |
| Voice sounds robotic             | You may be hearing the Android TTS fallback. Check internet connection for Edge TTS. |
| Speech is too fast or slow       | Adjust speech speed in Settings.                   |
| Arabic pronunciation is wrong    | This may occur on the Android TTS fallback. Edge TTS handles Arabic more accurately. |
| Recommendations not spoken       | They may still be loading. Wait a moment or tap the FAB to replay. |

---

## Next Steps

- Configure speech settings in [Settings](settings.md).
- Learn about [Accessibility features](accessibility.md) built for visually impaired users.
- Review how [AI Recommendations](ai-recommendations.md) are generated.
