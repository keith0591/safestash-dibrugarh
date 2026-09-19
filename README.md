# SafeStash Dibrugarh — website

A single self-contained `index.html` (inline CSS, one line of vanilla JS) for **SafeStash Dibrugarh**, a secure self-storage business at Rajakhat, Lahoal, Dibrugarh, Assam. No build step, no frameworks, no npm — open the file in a browser to preview it locally. The only assets are two JPEGs in `assets/`.

Live repo: <https://github.com/keith0591/safestash-dibrugarh>

## Enabling GitHub Pages

Go to **Settings → Pages**, set **Source** to *Deploy from a branch*, pick the **`main`** branch with the **`/ (root)`** folder, and click **Save**. GitHub builds the site in a minute or two and publishes it at `https://keith0591.github.io/safestash-dibrugarh/`. Because `index.html` sits at the repository root and pulls in no build artifacts, nothing else needs configuring. When you buy a domain, add it under **Settings → Pages → Custom domain** (which commits a `CNAME` file for you) and point a DNS `A`/`CNAME` record at GitHub Pages.

Or enable it from the command line:

```bash
gh api repos/keith0591/safestash-dibrugarh/pages -X POST -f 'source[branch]=main' -f 'source[path]=/'
```

## Still to replace

| Where | Currently | Replace with |
|---|---|---|
| `<link rel="canonical">`, `og:url`, `og:image`, `twitter:image`, JSON-LD `url` / `image` / `logo` / `@id` (8 spots) | `https://REPLACE-WITH-YOUR-DOMAIN.com/` | Your real domain once purchased. Until then the Open Graph image won't resolve, so link previews stay blank — see note below. |
| Map `<iframe src>` | The resolved Google embed for your live Business Profile (Knowledge Graph id `/g/11zxmqrhc4`) | Nothing — this already points at your real listing. Swap only if you move premises. |

**Open Graph images need an absolute URL**, which is why those tags still point at the placeholder domain. If you enable Pages before buying a domain and want link previews working in the meantime, replace `https://REPLACE-WITH-YOUR-DOMAIN.com/` with `https://keith0591.github.io/safestash-dibrugarh/` everywhere:

```bash
sed -i 's|https://REPLACE-WITH-YOUR-DOMAIN.com/|https://keith0591.github.io/safestash-dibrugarh/|g' index.html
```

## Contact details, if they ever change

The phone number appears in 12 places — six `wa.me/916003632998` links, six `tel:+916003632998` links — plus the JSON-LD `telephone` field and the visible text "+91 60036 32998". To change it:

```bash
sed -i 's/916003632998/91XXXXXXXXXX/g; s/+91 60036 32998/+91 XXXXX XXXXX/g' index.html
```

## Editing content

- **Prices** live in the `<tbody>` of the pricing table *and* in the `hasOfferCatalog` JSON-LD block in `<head>` — update both so search results stay accurate.
- **Brand colours** are CSS custom properties at the top of the `<style>` block (`--navy`, `--orange`, `--cream`).
- **Opening hours** appear in the Location section and in the JSON-LD `openingHoursSpecification`.
- **Coordinates** in the JSON-LD `geo` block (27.4557435, 94.9903042) were read out of your live Google Business Profile, so they match the pin Google already has.
- **Images:** `assets/storefront.jpg` (1100×1100) is used both as the hero background, behind a navy overlay, and as the photo in the Location section. `assets/og-image.jpg` (1200×630) is the social-share card — it is never shown on the page itself, only by WhatsApp, Facebook and X when someone pastes the link. Both are generated from the original `SafeStashCoverPhoto.PNG`.

## What's included

- Semantic HTML (`<header>`, `<main>`, `<section>`, `<footer>`), mobile-first responsive layout
- `schema.org` **SelfStorage** JSON-LD (a valid LocalBusiness subtype — `StorageFacility` is not a schema.org type) with address, offers and hours
- Title, meta description, Open Graph and Twitter Card tags targeting "self storage Dibrugarh" / "storage Lahoal Assam" (both the *Lahoal* and *Lahowal* spellings are covered in the keywords)
- Inline SVG shield-and-padlock logo and a matching SVG favicon — no logo file to load
- `tel:` and `wa.me` links throughout, so phones open the dialler and the WhatsApp app directly; a sticky Call + WhatsApp bar on mobile
- Pricing table reflows into cards on narrow screens
- Two external requests: the Playfair Display webfont from Google Fonts (headings fall back to Georgia if blocked) and nothing else — all images are local
