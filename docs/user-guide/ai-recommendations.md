# AI Recommendations

After scanning a product, the app uses artificial intelligence to suggest
**three alternative products** that may be safer or healthier for you. This
page explains how recommendations work and how to get the most out of them.

---

## How It Works

The AI recommendation engine is powered by **Groq AI** and considers three
main factors when selecting alternatives:

1. **Allergen Safety** — Products that conflict with your dietary profile are
   excluded. The AI maintains a **banned ingredient list** based on your
   specific restrictions.
2. **Dietary Profile Match** — Alternatives are chosen to align with your
   dietary preferences (vegan, gluten-free, dairy-free, etc.).
3. **Health Considerations** — Products with excessively high calories, fat,
   or sodium are deprioritized in favor of healthier options.

!!! warning
    The AI uses a strict BANNED list to prevent dangerous suggestions. For
    example, if you have a **nut allergy**, the AI will never recommend
    almond milk, peanut butter, or any product containing tree nuts or
    peanuts. This banned list is generated directly from your dietary profile.

---

## What You See

After a scan, the AI recommendations section appears below the product details.
Each recommendation card shows:

| Field              | Description                                                        |
|--------------------|--------------------------------------------------------------------|
| **Brand Name**     | The brand or manufacturer of the alternative product.              |
| **Arabic Name**    | The product name in Arabic for bilingual identification.           |
| **Reason**         | A short explanation of why this product is recommended.            |
| **Match Percentage** | A score (e.g., 92%) showing how well the product fits your profile. |

### Example Recommendation Card

> **Brand:** Oatly  
> **Arabic Name:** حليب الشوفان  
> **Reason:** Dairy-free oat milk with lower calories and no allergens matching your profile.  
> **Match:** 95%

---

## Viewing Full Nutrition Details

You can tap on any recommendation card to open a **bottom sheet** with the
full nutritional breakdown of that alternative product. The bottom sheet
includes:

- Complete nutrition grid (calories, fat, protein, carbs, sodium, fiber, sugar).
- Ingredient list for the recommended product.
- Allergen information.
- A **Speak Aloud** button to hear the nutrition details read out loud.

!!! tip
    Use the Speak Aloud button in the bottom sheet if you want to hear the
    nutritional comparison without looking at the screen. This is especially
    helpful when comparing products side by side in a store.

---

## Match Percentage Explained

The match percentage reflects how well the recommended product aligns with
your needs. Here is a general guide:

| Match Range | Meaning                                                            |
|-------------|--------------------------------------------------------------------|
| **90-100%** | Excellent fit. Meets all dietary requirements and is a healthy choice. |
| **75-89%**  | Good fit. Meets most requirements with minor trade-offs.            |
| **60-74%**  | Acceptable fit. Some aspects may not perfectly match your profile.   |
| **Below 60%** | Rare. The AI generally avoids suggesting low-match products.       |

---

## Internet Requirements

AI recommendations require an active internet connection to communicate with
the Groq AI service.

### When You Are Online

- Recommendations load automatically after a product scan.
- Results typically appear within a few seconds.

### When You Are Offline

- A message appears: **"AI Recommendations require internet"**.
- Previously loaded recommendations are **cached** and remain available for
  products you have already scanned.
- You can still view all other product details (nutrition, ingredients,
  allergens) without internet.

!!! note
    If recommendations fail to load due to a connection issue, a **Retry**
    button will appear. Tap it once your internet connection is restored to
    fetch the recommendations.

---

## How the Banned List Works

The AI does not just look for good matches — it actively avoids dangerous ones.
Based on your dietary profile, a banned list is generated:

| Dietary Restriction | Banned Ingredients (Examples)                                |
|---------------------|--------------------------------------------------------------|
| **Nut Allergy**     | Almonds, peanuts, cashews, walnuts, almond milk, peanut oil  |
| **Dairy-Free**      | Milk, cheese, whey, casein, butter, cream, lactose           |
| **Gluten-Free**     | Wheat, barley, rye, malt, flour, bread crumbs                |
| **Vegan**           | Meat, dairy, eggs, honey, gelatin, whey                      |
| **Low-Sodium**      | Products exceeding sodium thresholds per serving              |

!!! warning
    While the AI banned list is thorough, it is not a medical device. If you
    have a severe or life-threatening allergy, always verify product labels
    yourself and consult your healthcare provider.

---

## Tips for Better Recommendations

- **Keep your dietary profile up to date.** The AI relies entirely on the
  restrictions you set in your profile. If your needs change, update them
  in [Settings](settings.md).
- **Scan a variety of products.** The more products you scan, the better you
  will understand what alternatives are available in your area.
- **Check the match percentage.** Higher percentages mean stronger alignment
  with your profile.
- **Use the Speak Aloud button.** Listening to recommendations helps you
  compare products quickly while shopping.

---

## Troubleshooting

| Problem                             | Solution                                          |
|--------------------------------------|-------------------------------------------------|
| No recommendations appear            | Check your internet connection and tap Retry.    |
| Recommendations seem irrelevant      | Review your dietary profile in Settings.          |
| "AI Recommendations require internet" | Connect to Wi-Fi or mobile data, then tap Retry. |
| Recommendations load slowly          | This can happen on slow connections. Wait a few seconds. |

---

## Next Steps

- Learn how [Text-to-Speech](text-to-speech.md) reads recommendations aloud.
- Adjust your dietary profile in [Settings](settings.md).
- Review [Accessibility features](accessibility.md) for hands-free use.
