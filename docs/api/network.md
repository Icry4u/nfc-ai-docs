# API Reference — Network Layer

## GroqClient

**Path:** `network/GroqClient.java`

HTTP client for the Groq AI recommendation API.

### Configuration

| Constant | Value |
|----------|-------|
| `API_URL` | `https://api.groq.com/openai/v1/chat/completions` |
| `MODEL` | `llama-3.3-70b-versatile` |
| Temperature | 0.1 (deterministic) |
| Max tokens | 512 |
| Connect timeout | 15s |
| Read/Write timeout | 30s/15s |

### Key Methods

| Method | Description |
|--------|-------------|
| `getRecommendations(name, ingredients, allergens, profile, lang, cal, fat, callback)` | Sends prompt to Groq, parses JSON response |
| `buildPrompt(...)` | Constructs the multi-section prompt with safety rules |
| `parseResponse(json)` | Extracts Recommendation objects from Groq response |
| `sanitize(text)` | Strips non-Arabic/English characters |
| `hasGibberish(text)` | Detects CJK/unexpected scripts (>10% threshold) |

### Prompt Structure

The prompt sent to Groq has these sections:

1. **System message** — "You are a food safety assistant. Verify EACH recommendation..."
2. **Context** — Scanned product name, allergens, nutrition, user profile, ingredients
3. **Rule 1: Type Match** — Same category alternatives (snack→snack, milk→milk)
4. **Rule 2: No Allergens** — Product allergens + user profile restrictions + BANNED list + hidden allergen knowledge
5. **Rule 2b: Health** — Lower calories/fat if product is unhealthy (>400cal or >25g fat)
6. **Rules 3-10** — Real brands, matchPercent, JSON format, name/nameAr/reason fields, nutrition, ingredients

### BANNED List Generation

Dynamically generated from `userProfile`:

```java
if (userProfile.contains("nut")) {
    sb.append("BANNED: almond milk, cashew milk, hazelnut anything...");
}
if (userProfile.contains("dairy")) {
    sb.append("BANNED: any product with milk, cream, cheese, butter...");
}
```

### Safety Pipeline

```
User profile → BANNED list → Hidden allergen knowledge → VERIFY step
```

---

## EdgeTtsClient

**Path:** `network/EdgeTtsClient.java`

WebSocket client for Microsoft Edge neural TTS.

### Configuration

| Constant | Value |
|----------|-------|
| WSS URL | `wss://speech.platform.bing.com/consumer/speech/synthesize/readaloud/edge/v1` |
| `TRUSTED_CLIENT_TOKEN` | `6A5AA1D4EAFF4E9FB37E23D68491D6F4` |
| `SEC_MS_GEC_VERSION` | `1-143.0.3650.75` |
| Output format | `audio-24khz-48kbitrate-mono-mp3` |
| Timeouts | 5s connect, 5s read, 5s write |

### Voices

| Language | Voice | Gender |
|----------|-------|--------|
| Arabic (SA) | `ar-SA-ZariyahNeural` | Female |
| Arabic (SA) | `ar-SA-HamedNeural` | Male |
| English (US) | `en-US-JennyNeural` | Female |
| English (US) | `en-US-GuyNeural` | Male |

### DRM Token Generation

```java
private String generateSecMsGec() {
    long windowsTicks = (unixSeconds + WINDOWS_EPOCH_OFFSET) * TICKS_PER_SECOND;
    long rounded = windowsTicks - (windowsTicks % 3_000_000_000L);
    return SHA256(rounded + TRUSTED_CLIENT_TOKEN);
}
```

### Key Methods

| Method | Description |
|--------|-------------|
| `synthesize(text, voiceName, rate, callback)` | Opens WebSocket, sends SSML, collects MP3 bytes |
| `generateSecMsGec()` | Creates DRM authentication token |
| `generateMuid()` | Random machine ID for request |
| `cancel()` | Closes active WebSocket |

### WebSocket Flow

```
1. Connect with TrustedClientToken + Sec-MS-GEC + ConnectionId
2. Send config message (output format)
3. Send SSML with voice + prosody rate
4. Receive binary MP3 chunks
5. Detect "Path:turn.end" → combine chunks → callback.onAudioReady(bytes)
```

---

## AiRecommendationManager

**Path:** `network/AiRecommendationManager.java`

Orchestrates the recommendation pipeline.

### Recommendation Class

```java
public static class Recommendation {
    public String name;          // English brand name
    public String nameAr;        // Arabic description
    public String reason;        // Why it's recommended
    public int matchPercent;     // 70-95
    public int weightGrams;
    public float calories, protein, carbs, fat;
    public String ingredients, ingredientsAr;
}
```

### Flow

```
getRecommendations() called
  → Execute on background thread
  → GroqClient.getRecommendations()
    → onSuccess: cache results → callback.onSuccess()
    → onError: try cache → if no cache → callback.onError("no_internet")
```

---

## RecommendationCache

**Path:** `network/RecommendationCache.java`

SharedPreferences-based cache for offline recommendation access.

### Key Methods

| Method | Description |
|--------|-------------|
| `put(productName, recs)` | Serializes recommendations to JSON, stores by product key |
| `get(productName)` | Deserializes cached recommendations, returns null if not cached |

### Cache Key Format

```java
"recs_" + productName.toLowerCase().trim().replace(" ", "_")
// Example: "recs_almarai_full_fat_milk"
```
