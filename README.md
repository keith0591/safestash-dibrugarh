# SafeStash Dibrugarh — website

A single self-contained `index.html` (inline CSS, one line of vanilla JS) for **SafeStash Dibrugarh**, secure self storage at Rajakhat, Lahoal, Dibrugarh, Assam. No build step, no frameworks, no npm. The only assets are two JPEGs in `assets/`.

- Repo: <https://github.com/keith0591/safestash-dibrugarh>
- Domain: **safestashdibrugarh.in** (registered with Hostinger, not yet pointed here)
- Status: **built and validated, not yet live.** GitHub Pages is deliberately switched off.

Preview locally with `python3 -m http.server 8000` in this folder, then open <http://localhost:8000>. Prefer that over opening `index.html` directly — the Google Maps embed behaves like production over HTTP.

---

## Going live — the whole checklist

### 1. Point the domain at GitHub (do this first; DNS takes time to propagate)

In **Hostinger → Domains → safestashdibrugarh.in → DNS / Nameservers → Manage DNS records**, delete any existing `A` record for `@` (Hostinger usually parks one), then add:

| Type | Name | Points to | TTL |
|---|---|---|---|
| A | `@` | `185.199.108.153` | default |
| A | `@` | `185.199.109.153` | default |
| A | `@` | `185.199.110.153` | default |
| A | `@` | `185.199.111.153` | default |
| CNAME | `www` | `keith0591.github.io` | default |

All four `A` records are needed — they're GitHub's load-balanced Pages servers. Leave any `MX` records for email alone.

### 2. Turn on GitHub Pages

**Settings → Pages → Source: Deploy from a branch → `main` / `(root)` → Save.** Or:

```bash
gh api repos/keith0591/safestash-dibrugarh/pages -X POST -f 'source[branch]=main' -f 'source[path]=/'
```

The `CNAME` file in this repo already contains `safestashdibrugarh.in`, so Pages picks up the custom domain automatically — you shouldn't need to type it into the Pages settings.

### 3. Wait for the certificate, then force HTTPS

Once DNS resolves, GitHub issues a Let's Encrypt certificate (usually minutes, occasionally up to 24 hours). When **Settings → Pages** stops showing a certificate warning, tick **Enforce HTTPS**. Check both `safestashdibrugarh.in` and `www.safestashdibrugarh.in` load.

### 4. Tell Google about it

- In your **Google Business Profile**, set the website field to `https://safestashdibrugarh.in` — this is the single highest-value SEO action available, because the profile is already live and carries your real pin.
- Optionally add the site to [Google Search Console](https://search.google.com/search-console) and submit the URL for indexing.

### 5. Check the link preview

Paste the URL into a WhatsApp chat with yourself. You should see the storefront photo with the title and description, not a bare link. If it's blank, the Open Graph tags are fine but the domain isn't resolving yet — wait for DNS.

---

## How the contact details are wired

The number appears in **13 places**: six `wa.me/916003632998` links, six `tel:+916003632998` links, the JSON-LD `telephone` field, plus the visible text "+91 60036 32998". To change it:

```bash
sed -i 's/916003632998/91NEWNUMBER00/g; s/+91 60036 32998/+91 XXXXX XXXXX/g' index.html
```

WhatsApp links use the `wa.me` format, which opens the WhatsApp app directly on a phone and WhatsApp Web on a desktop. `tel:` links open the dialler. Both are prefilled — the WhatsApp message reads "Hi, I'd like to know more about storage space at SafeStash".

## Editing content

- **Prices** live in the `<tbody>` of the pricing table *and* in the `hasOfferCatalog` JSON-LD block in `<head>`. Update both, or search results will disagree with the page.
- **Brand colours** are CSS custom properties at the top of the `<style>` block (`--navy`, `--orange`, `--cream`).
- **Opening hours** appear in the Location section and in the JSON-LD `openingHoursSpecification`.
- **Coordinates** in the JSON-LD `geo` block (27.4557435, 94.9903042) were read out of the live Google Business Profile, so they match the pin Google already holds. The map `<iframe>` resolves against that same listing (Knowledge Graph id `/g/11zxmqrhc4`) — no need to touch it unless you move premises.
- **Images:** `assets/storefront.jpg` (1100×1100) is the hero background, behind a navy overlay, and the photo in the Location section. `assets/og-image.jpg` (1200×630) is the social-share card — never shown on the page, only by WhatsApp, Facebook and X when someone pastes the link. Both were generated from the original `SafeStashCoverPhoto.PNG`; regenerate them with Pillow if you reshoot.

## SEO notes

On-page work is done: one `<h1>` and a clean `h2`/`h3` hierarchy, a 58-character title and a 149-character description (both inside what Google displays), `SelfStorage` JSON-LD carrying the real coordinates and linked to the Business Profile via `sameAs`, `robots.txt`, `sitemap.xml`, descriptive image `alt` text, and a preloaded hero so the largest element paints early. First load is ~156 KB.

Two things matter more than anything in this repo:

1. **The Google Business Profile is the ranking engine for local search**, not the website. Put `https://safestashdibrugarh.in` in its website field, keep hours accurate, post photos, and ask early customers for reviews. A handful of genuine reviews will outrank any on-page tuning.
2. **Reviews and ratings are deliberately absent from the structured data.** `aggregateRating` markup without real reviews behind it is a manual-action risk. Once you have genuine Google reviews, they surface through the Business Profile anyway.

Worth doing later, once there's traffic to justify it: split the page into separate URLs for the distinct searches people actually make — student luggage storage, two-wheeler storage, shop inventory storage. A single page can only rank for so many phrases at once. `FAQPage` markup was considered and skipped: Google restricted FAQ rich results to government and health sites in 2023, so it would add markup for no gain.

## What's included

- Semantic HTML (`<header>`, `<main>`, `<section>`, `<footer>`), mobile-first responsive layout, verified with no horizontal overflow down to 345px wide
- `schema.org` **SelfStorage** JSON-LD — a valid LocalBusiness subtype, and the one matching Google's own category for this listing (`gcid:self_storage_facility`). `StorageFacility` is not a schema.org type.
- Title, meta description, Open Graph and Twitter Card tags targeting "self storage Dibrugarh" / "storage Lahoal Assam" (both the *Lahoal* and *Lahowal* spellings are in the keywords)
- Inline SVG shield-and-padlock logo matching the SafeStash mark, plus a matching SVG favicon — no logo file to load
- Sticky Call / WhatsApp bar on mobile; pricing table reflows into cards on narrow screens
- One external request: the Playfair Display webfont from Google Fonts, with Georgia as the fallback. Everything else is local.
