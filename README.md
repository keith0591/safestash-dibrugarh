# SafeStash Dibrugarh — website

A single self-contained `index.html` (inline CSS, one line of vanilla JS) for **SafeStash Dibrugarh**, a secure self-storage business in Lahowal, Dibrugarh, Assam. No build step, no frameworks, no npm — open the file in a browser to preview it locally.

## Enabling GitHub Pages

Push this repo to GitHub, then go to **Settings → Pages**, set **Source** to *Deploy from a branch*, pick the **`main`** branch with the **`/ (root)`** folder, and click **Save**. GitHub builds the site in a minute or two and publishes it at `https://<your-username>.github.io/<repo-name>/`. Because `index.html` sits at the repository root and pulls in no build artifacts, nothing else needs configuring. When you buy a domain, add it under **Settings → Pages → Custom domain** (which commits a `CNAME` file for you) and point a DNS `A`/`CNAME` record at GitHub Pages.

## Placeholders to replace before going live

| Where | Placeholder | Replace with |
|---|---|---|
| Every WhatsApp link (`wa.me/91XXXXXXXXXX`, 5 occurrences) | `91XXXXXXXXXX` | Your number in country-code + number form, no `+` or spaces — e.g. `919876543210` |
| Map `<iframe src>` in the Location section | A generic "Lahowal, Dibrugarh" map | The embed URL from Google Maps → your Business Profile → **Share → Embed a map** → copy the `src="…"` value |
| `<link rel="canonical">`, `og:url`, `og:image`, `twitter:image`, and the JSON-LD `url` / `image` / `@id` | `https://REPLACE-WITH-YOUR-DOMAIN.com/` | Your real domain once purchased |
| JSON-LD `telephone` | `+91-XXXXXXXXXX` | Your business phone number |
| JSON-LD `geo` latitude/longitude | Approximate Lahowal coordinates | Exact coordinates from your Google Business Profile |

Swapping the WhatsApp number in one shot:

```bash
sed -i 's/91XXXXXXXXXX/919876543210/g' index.html
```

Also add a real **`og-image.jpg`** (1200×630, a photo of the facility with the wordmark) to the repo root — it's what shows up as the preview card when the link is shared on WhatsApp or Facebook.

## Editing content

- **Prices** live in the `<tbody>` of the pricing table *and* in the `hasOfferCatalog` JSON-LD block in `<head>` — update both so search results stay accurate.
- **Brand colours** are CSS custom properties at the top of the `<style>` block (`--navy`, `--orange`, `--cream`).
- **Opening hours** appear in the Location section and in the JSON-LD `openingHoursSpecification`.

## What's included

- Semantic HTML (`<header>`, `<main>`, `<section>`, `<footer>`), mobile-first responsive layout
- `schema.org` **SelfStorage** JSON-LD (a valid LocalBusiness subtype — `StorageFacility` is not a schema.org type) with address, offers and hours
- Title, meta description, Open Graph and Twitter Card tags targeting "self storage Dibrugarh" / "storage Lahowal Assam"
- Inline SVG shield-and-padlock logo and an SVG favicon — no image files to load
- Sticky WhatsApp call-to-action bar on mobile; the pricing table reflows into cards on narrow screens
- Only one external request: the Playfair Display webfont from Google Fonts (headings fall back to Georgia if it's blocked)
