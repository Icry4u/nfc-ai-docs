# Scanning Products

The core feature of NFC-AI: Smart Shopping Assistant is scanning food products
using your phone's NFC sensor. This page explains how scanning works and what
you will see after a successful scan.

---

## How to Scan

1. **Make sure NFC is enabled** on your Android device. You can check this in
   your phone's system settings under "Connected devices" or "NFC".
2. **Open the app** so you are on the main scanner screen.
3. **Hold the back of your phone** near the NFC tag on the product. The NFC
   antenna on most phones is located in the upper-center of the back panel.
4. **Keep your phone steady** for about one second until the app responds.

!!! tip
    Hold your phone within 1-2 centimeters of the NFC tag. If you are unsure
    where your phone's NFC antenna is, slowly move the phone across the tag
    until you feel a response.

---

## What Happens on a Successful Scan

When the app successfully reads the NFC tag, you will experience:

- A **chime sound** plays to confirm the scan.
- A short **vibration** (haptic feedback) pulses through your phone.
- The app navigates to the **Product Detail Screen** with full product information.

## What Happens on a Failed Scan

If the scan does not work, you will hear:

- An **error tone** indicating the tag could not be read.

Common reasons for a failed scan include:

| Issue                        | Solution                                                |
|------------------------------|---------------------------------------------------------|
| Phone too far from tag       | Move your phone closer, within 1-2 cm.                  |
| NFC disabled on phone        | Enable NFC in your phone's system settings.              |
| Tag is damaged               | Try a different product with a working NFC tag.          |
| Phone case too thick         | Remove bulky cases that may block the NFC signal.        |
| Incompatible tag type        | The app works with **NTAG215** tags specifically.        |

---

## NFC Tag Technology

The app uses **NTAG215** NFC tags attached to products. Unlike many NFC apps that
read NDEF data stored on the tag, this app uses a **UID-based lookup** system.
Each tag has a unique identifier (UID) that the app matches against a product
database.

!!! note
    Because the app uses UID-based lookup rather than NDEF data, the NFC tags
    themselves do not need to contain any written data. They only need to be
    valid NTAG215 tags with a readable UID.

---

## Background Scanning

The app supports **background NFC scanning**. This means:

- If the app is **minimized** (running in the background) and you scan an NFC tag,
  the app will come to the foreground and show the product immediately.
- The splash screen animation is **skipped** during background scans for a faster
  experience.
- You go directly to the product detail screen.

!!! tip
    Background scanning is useful when you are in a store and quickly tapping
    multiple products. You do not need to have the app in the foreground each time.

---

## Simulate Scan (Testing Mode)

If you do not have physical NFC tags available, or your device does not have NFC
hardware, you can use the **Simulate Scan** button.

- This button is visible on the main scanner screen.
- Tapping it loads a sample product as if you had scanned a real NFC tag.
- The full product detail experience plays out, including sound, vibration,
  nutrition data, and AI recommendations.

!!! note
    The simulate scan feature is intended for testing and demonstration purposes.
    In a real shopping scenario, you would scan actual NFC tags on products.

---

## Product Detail Screen

After a successful scan (or simulated scan), the product detail screen displays
comprehensive information about the product.

### Product Header

- A large **emoji** representing the product category (for example, a milk carton
  emoji for dairy products).
- The **product name** in your selected language.

### Nutrition Grid

A grid layout showing key nutritional values per serving:

| Nutrient        | Example Value |
|-----------------|---------------|
| Calories        | 150 kcal      |
| Total Fat       | 8 g           |
| Saturated Fat   | 3 g           |
| Carbohydrates   | 18 g          |
| Sugar           | 12 g          |
| Protein         | 5 g           |
| Sodium          | 200 mg        |
| Fiber           | 2 g           |

### Ingredients List

A complete list of ingredients as found on the product packaging, displayed in
a readable format.

### Allergen Badges

Allergens are shown as colored badges:

- **Red badges** indicate allergens that conflict with your dietary profile.
- **Green badges** indicate allergens present in the product that do not conflict
  with your profile.

!!! warning
    Always double-check the physical product label in addition to the app.
    While the database is kept accurate, the app should be used as a helpful
    tool alongside — not a replacement for — reading product labels.

### AI Recommendations

Below the product details, you will find AI-powered alternative product
suggestions. See the [AI Recommendations](ai-recommendations.md) page for
full details on this feature.

---

## Scan History

Every product you scan is saved to your **scan history**, accessible from
the main screen. This lets you review previously scanned items without
needing to scan them again.

---

## Next Steps

- Understand how [AI Recommendations](ai-recommendations.md) work.
- Learn about [Text-to-Speech](text-to-speech.md) for hands-free product info.
- Customize your experience in [Settings](settings.md).
