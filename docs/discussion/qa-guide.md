# Q&A Guide — Discussion Prep

Straightforward answers to the questions you'll likely get asked. Keep it natural — you built this, you know it.

<div style="margin:24px 0;padding:20px;background:#e0f2f1;border-radius:12px;border:1px solid #00897b;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px;">
  <div>
    <div style="font-weight:700;color:#004d40;font-size:1rem;">Ready to test yourself?</div>
    <div style="color:#00695c;font-size:0.88rem;margin-top:4px;">40 MCQ questions — simulates the real committee discussion</div>
  </div>
  <a href="../quiz.html" style="background:#00897b;color:#fff;padding:12px 24px;border-radius:8px;font-weight:600;font-size:0.9rem;text-decoration:none;white-space:nowrap;">Launch Quiz →</a>
</div>

---

## General

### "What does your app do?"

It's a shopping assistant for people with food allergies. You tap your phone on a product's NFC tag, and the app shows you nutrition info, warns you about allergens based on your profile, and suggests safe alternatives using AI. Everything gets read aloud for visually impaired users.

### "Why this project?"

Food allergies are a real safety issue — misreading a label can send someone to the hospital. We wanted to make grocery shopping safer, especially for people who can't easily read small ingredient labels.

### "What makes it different from other food apps?"

- **NFC instead of barcodes** — works by touch, no camera needed, doesn't depend on lighting, and barcodes wear off or get damaged
- **AI safety verification** — the AI checks every suggestion against the user's full allergy profile before showing it
- **Neural TTS** — reads everything aloud in natural-sounding Arabic or English, not a robotic voice

---

## Technical

### "What's the architecture?"

The app is organized in layers — each part has one job:

- **Screens** show information and react to what the user does
- **Logic layer** sits in the middle — handles what happens when you tap something, survives screen rotation without losing data
- **Data layer** talks to the local database and the internet (Groq AI, Edge TTS)

Think of it like a restaurant: the **waiter** (screen) takes your order, passes it to the **kitchen** (logic), which gets ingredients from the **storage** (database) or orders from outside (internet).

### "Why did you structure it this way?"

So each part can be changed without breaking the others. If we swap the AI provider, only the network part changes — the screens don't need to know about it. It also means rotating the phone or switching languages doesn't reset anything.

### "How does NFC scanning work?"

You tap your phone on the NFC sticker attached to a product. The phone reads a unique ID from the sticker — like a serial number burned into the chip at the factory. We look that ID up in our database and show the matching product. If it's not in the database, you hear an error sound.

No data needs to be written to the sticker — any blank NFC sticker works.

### "Why NFC and not barcodes?"

- Works by touch — no camera aiming needed, which matters for visually impaired users
- Works in any lighting
- Barcodes tear, fade, or get covered — NFC chips are inside the sticker and don't wear out
- Some phones struggle to read barcodes reliably

### "How do AI recommendations work?"

When you scan a product, we send a message to an AI model (Groq) that says: "This person scanned this product. They can't eat X, Y, Z. Give me 3 safe alternatives." We also give it a list of banned ingredients based on the user's profile so it can't suggest something dangerous. The AI checks its own answer before sending it back.

### "Why Groq?"

It's free — no credit card, no cost. It responds in under 2 seconds and handles Arabic well. We tried Google's AI first but kept running out of free calls during testing.

### "How does the voice readout work?"

When you scan a product, the app reads everything aloud — product name, nutrition, ingredients, allergens, then recommendations. It uses Microsoft's neural voice service for natural-sounding Arabic and English. If there's no internet, it switches to the phone's built-in voice automatically.

### "What if there's no internet?"

- All 15 products are stored on the phone — scanning still works
- AI recommendations from previous scans are saved locally — if you scanned this product before, the cached result is shown
- If neither is available, a retry button appears

### "How does Arabic work?"

Everything in the app has an Arabic translation. Arabic text is stored with pronunciation marks (like vowel signs) so the voice reads it correctly, but those marks are hidden in the UI so the text looks clean. The layout automatically flips right-to-left when Arabic is selected.

---

## Design

### "Why a custom tutorial instead of a library?"

We built it from scratch — it shows we understand custom Views, canvas drawing, and touch interception. Zero extra dependencies, and we get full control over the spotlight shape and behavior.

### "Where is user data stored?"

Everything stays on the device — SharedPreferences for settings, Room database for products and scan history. The only external calls are Groq (for AI) and Edge TTS (for voice). No data goes to our servers.

### "Is the API key secure?"

For this prototype, it's embedded in the app. In production, we'd move it behind a backend server and use Android Keystore for local secrets.

---

## Future Improvements

If asked "What would you improve?":

1. **Barcode scanning** — camera-based backup for stores without NFC tags
2. **Open Food Facts** — real product database for verified nutrition data (we tested the API, it has Saudi products)
3. **Favorites** — save frequently checked products
4. **Multi-user profiles** — family mode with separate allergy profiles per member
