# Accessibility Features

NFC-AI: Smart Shopping Assistant was designed with accessibility as a core
priority. Many features are built specifically to support **visually impaired
users**, but they benefit everyone. This page describes the accessibility
features available throughout the app.

---

## Text-to-Speech with Neural Voice

The app uses **Edge TTS neural voices** to produce natural, human-like speech.
This is a significant improvement over robotic-sounding synthesized voices.

- Neural voices have natural intonation, rhythm, and emphasis.
- Both **Arabic** and **English** are supported with high-quality voices.
- Product information is read in a logical order: name, nutrition, ingredients,
  allergens, and recommendations.
- If a conflicting allergen is detected, the **allergen warning is spoken first**
  as an immediate safety alert.

!!! tip
    If you rely on TTS as your primary way of receiving product information,
    enable **Auto TTS** in Settings so every scan is read aloud automatically.

### Arabic Pronunciation with Tashkeel

Arabic text-to-speech includes **tashkeel** (diacritical marks such as fatha,
kasra, damma, and sukun) to ensure correct pronunciation. These marks indicate
short vowels and other pronunciation details essential for accurate Arabic
speech.

- Tashkeel is **included in the text sent to the speech engine** for precise
  pronunciation.
- Tashkeel is **stripped from the on-screen display** to keep the interface
  clean and readable.

This means you hear correctly pronounced Arabic while the visual text remains
uncluttered — the best of both approaches.

---

## Haptic Feedback

The app uses vibration to communicate scan results through touch:

- **Successful scan** — A short vibration pulse confirms the NFC tag was read.
- **Failed scan** — A different vibration pattern indicates an error.

Haptic feedback works alongside audio cues (chime for success, error tone for
failure), giving you two forms of non-visual confirmation for every scan.

!!! note
    Haptic feedback relies on your phone's vibration motor. Make sure vibration
    is enabled in your phone's system settings for this feature to work.

---

## High-Contrast Allergen Badges

Allergen information is displayed using **color-coded badges** designed for
maximum visibility:

| Badge Color  | Meaning                                                          |
|--------------|------------------------------------------------------------------|
| **Red**      | **Danger.** This allergen conflicts with your dietary profile.    |
| **Green**    | **Safe.** This allergen is present but does not conflict with your profile. |

The badges use high-contrast colors that are easy to distinguish, even for
users with reduced vision. The text on each badge clearly names the allergen.

!!! warning
    Red allergen badges indicate a direct conflict with your dietary profile.
    If you see a red badge (or hear an allergen warning via TTS), exercise
    caution with that product.

---

## RTL and Arabic Support

The app fully supports **right-to-left (RTL) layout** for Arabic language users:

- All text, layouts, and navigation elements are mirrored appropriately when
  Arabic is selected as the language.
- **Bidirectional (BiDi) text** is handled correctly, meaning mixed Arabic and
  English content (such as brand names in English within Arabic text) displays
  in the proper reading order.
- Product names, ingredients, and allergens appear in the correct direction
  based on the language.

!!! note
    When you switch the app language to Arabic, the entire interface flips to
    RTL layout. Buttons, menus, and navigation all adjust to follow natural
    Arabic reading direction (right to left).

---

## Large Font Option

The app offers three font size settings: **Small**, **Medium**, and **Large**.

The **Large** font size is specifically recommended for users with low vision:

- All text throughout the app scales up, including product names, nutrition
  values, ingredient lists, and allergen badges.
- Button labels and menu items also increase in size.
- The layout adapts to accommodate larger text without cutting off information.

Change the font size in **Settings > Font Size**.

!!! tip
    Combine the Large font setting with the Dark theme for the highest
    readability in most lighting conditions.

---

## Dark Mode

Dark mode reduces screen brightness and uses light text on dark backgrounds:

- **Reduces eye strain** in low-light environments.
- **Improves readability** for users with light sensitivity.
- **Saves battery** on phones with OLED screens.

Three theme options are available:

| Theme       | Behavior                                                          |
|-------------|-------------------------------------------------------------------|
| **System**  | Matches your phone's system-wide dark/light setting automatically. |
| **Light**   | Always uses a light background.                                    |
| **Dark**    | Always uses a dark background with light text.                     |

Change the theme in **Settings > Theme**.

---

## Speech Speed Control

Users who need more time to process spoken information can slow down the
TTS voice:

| Speed    | Use Case                                                           |
|----------|--------------------------------------------------------------------|
| **x0.5** | Best for users who need extra time to process each piece of information. |
| **x1.0** | Standard speed for general use.                                    |
| **x1.5** | For experienced users who are comfortable with faster speech.      |

Change the speed in **Settings > Speech Speed**.

---

## Speak Aloud Button (Always Available)

The floating action button (FAB) for speaking product information is **always
accessible** on the product detail screen, regardless of other settings:

- It works even when Auto TTS is turned off.
- It provides an on-demand way to hear all product information.
- It is positioned in a consistent, easy-to-find location on screen.

---

## Accessibility Summary

| Feature                    | Benefit                                              |
|----------------------------|------------------------------------------------------|
| Neural TTS voices          | Natural speech that is easy to understand.            |
| Arabic tashkeel            | Correct Arabic pronunciation without visual clutter.  |
| Haptic feedback            | Non-visual scan confirmation through vibration.       |
| High-contrast badges       | Clear red/green allergen indicators.                  |
| RTL Arabic layout          | Proper right-to-left interface for Arabic speakers.   |
| BiDi text support          | Mixed Arabic/English text displays correctly.         |
| Large font option          | Scalable text for low-vision users.                   |
| Dark mode                  | Reduced glare and improved readability.               |
| Speech speed control       | Adjustable pace for listening comfort.                |
| Allergen-first TTS         | Safety warnings spoken before other content.          |
| Speak Aloud FAB            | On-demand TTS available at all times.                 |

---

## Tips for Visually Impaired Users

- Enable **Auto TTS** so every scan is read aloud without needing to find and
  tap a button.
- Set speech speed to **x0.5** or **x1.0** until you are familiar with the
  information structure.
- Use the **Large** font size and **Dark** theme together for best on-screen
  visibility.
- Rely on **haptic feedback** to confirm successful scans without looking at
  the screen.
- Keep your **dietary profile accurate** so allergen warnings are always
  relevant to your needs.

---

## Next Steps

- Set up your preferences in [Settings](settings.md).
- Learn the basics in [Getting Started](getting-started.md).
- Explore [Text-to-Speech](text-to-speech.md) configuration options.
