# Technical Decisions

The major choices we made and why. If someone asks "why didn't you use X?" — the answer is here.

---

## NFC UID vs NDEF Data

**Chose: UID lookup.** Every NFC chip has a unique hardware ID burned in at the factory. We just map that ID to a product in our database. No need to write data to tags — any blank $0.10 NFC sticker works. NDEF would require programming each tag individually, which adds a step we don't need.

---

## Groq vs Gemini vs OpenAI

**Chose: Groq (llama-3.3-70b).** It's free (no credit card), fast (~1.5s responses), and handles Arabic well. We started with Gemini but hit daily quota limits during development. OpenAI requires a paid account.

---

## Edge TTS vs Google Cloud TTS vs Android TTS

**Chose: Edge TTS (primary) + Android TTS (fallback).** Edge TTS gives us neural-quality voices for free — same voices as Microsoft Edge browser. Google Cloud TTS costs money. Android's built-in TTS works offline but sounds robotic, so we use it as a fallback when there's no internet.

---

## Room DB vs Firebase vs REST API

**Chose: Room (local SQLite).** We have 15 pre-seeded products. A local database is instant, works offline, and needs no server. Firebase would add a network dependency for something that doesn't need it.

---

## Single Activity vs Multi-Activity

**Chose: Single Activity + Navigation Component.** All screens are Fragments managed by a NavHostFragment. This is Google's recommended approach — fragments share ViewModel data easily, transitions are smoother, and NFC intent handling is simpler with one activity.

---

## Custom Spotlight Tutorial vs Library

**Chose: Custom SpotlightOverlayView.** Built from scratch. Demonstrates that we understand custom Views and canvas drawing (which matters for a senior project). Zero dependencies, full control over shape, animation, and touch blocking.

---

## Tashkeel (Arabic Diacritics) Strategy

**Problem:** Arabic TTS needs diacritics for correct pronunciation, but they make the UI look cluttered.

**Solution:** Store text WITH diacritics in the database. Strip them for UI display, keep them for TTS. One source of truth, two output methods.

---

## AI Safety Pipeline

**Problem:** AI models can suggest dangerous products (like almond milk for someone with a nut allergy).

**Solution:** Multi-layer safety — system message tells the AI it's a food safety assistant, a BANNED list generated from the user's profile blocks known dangerous products, a hidden allergen knowledge base catches non-obvious allergens (whey is dairy, etc.), and a VERIFY step forces the AI to double-check before responding. App-side sanitization catches any remaining issues.

---

## Recommendation Caching

**Problem:** AI needs internet. What if the user scans the same product offline?

**Solution:** Cache Groq's results in SharedPreferences after each successful call. Offline? Serve the cached version. Never cached? Show "requires internet" with a retry button. Simple and effective for 15 products.

---

## Open Food Facts Evaluation

We tested the Open Food Facts API for real product data. Saudi Arabia has some coverage, but plant-based alternatives (the most common recommendations) had zero entries. Data quality was inconsistent — missing allergen tags, incomplete nutrition. We kept it as a future enhancement. The current Groq-only system is more reliable for the prototype.
