# Brand Partnerships

Willpower's brand-side partnership page: *"Get your brand in the room."*

An invitation page for prospective brand partners, covering the three ways to partner, sponsorship tiers per event series, the brands who have sent product, and proof from past activations.

**Live:** https://jayemsu.github.io/brandpartnerships/

Hosted on a personal account for review. To be transferred to the Willpower-HQ org once approved, after which the URL becomes `https://willpower-hq.github.io/brandpartnerships/` (update the canonical and Open Graph URLs in `index.html` at that point).

Companion to the [Speaker Recruitment page](https://willpower-hq.github.io/speakerrecruitment/) and built to match it.

## Status

**Awaiting Bill's approval before Pages is switched on.** Everything here is deploy-ready. Open items before launch:

- Sponsorship pricing is labelled draft on the page. Confirm the numbers per series.
- Real positioning copy and logo files for **Avro**, **Magic Milk**, and **Celzo** (current descriptions are drafts).
- The apply CTA points at `willpowerhq.com/apply?intent=sponsor`. Confirm or swap for the real form URL.

## Sections

| Section | What it shows |
|---|---|
| Hero | Video backdrop, headline, and live-counting highlight stats |
| Logo wall | Scrolling marquee of past partner brands |
| Ways to Partner | Three-path selector: sponsor, activate, send product |
| Brand Activations | The premium path, with click-to-expand feature cards |
| Brands Who Sent Products | 15 brands ordered by revenue, 8 per page, each opening a background and event gallery |
| Sponsorship Tiers | Tabbed per event series, plus bundles, custom mix, and a "Why it works" expander |
| What Partners Say | Testimonials from past participants |
| FAQ | The five most-asked partner questions |
| Find your place in the room | Closing call to action |

## Files

| File | Purpose |
|---|---|
| `index.html` | The landing page. A Claude Design component (an `x-dc` template plus a logic class). |
| `support.js` | The Design runtime. Parses the template, loads React from a CDN, and boots the page. |
| `assets/brands/` | Per-brand event photography for the product-sampling cards. |
| `.nojekyll` | Tells GitHub Pages to serve the files verbatim (no Jekyll processing). |

`index.html` and `support.js` must stay together in the same folder. The runtime is a separate file on purpose: `support.js` contains literal `&lt;x-dc&gt;` markers in its own source, so inlining it into the HTML breaks the runtime's self-parse.

Logos, the hero video, and venue photography load from `willpowerhq.com/assets/`, so they stay in sync with the main site. Only event photography specific to this page is committed here.

## Running locally

The runtime re-fetches the page over the network, so open it through a web server, not straight from disk (`file://` will not fully work).

```bash
python3 -m http.server 8000
# then open http://localhost:8000/
```

## Editing content

The data-driven sections are plain arrays inside `renderVals()` in the logic class at the bottom of `index.html`.

| Array | Controls |
|---|---|
| `brands` | The Brands Who Sent Products cards |
| `revData` | Revenue range per brand. This is what sets the card order |
| `noLogoBrands` | Brands with no logo file yet, rendered as a name instead |
| `tierData` | Sponsorship tiers per event series |
| `faqData` | FAQ questions and answers |
| `testimonials` | Partner quotes |

### Add a brand

1. Add `assets/brands/<brand-slug>/1.jpg`, `2.jpg`, `3.jpg`. **`1.jpg` is always the card cover.** Two images is fine; the gallery renders what exists.
2. Append an entry to `brands`:

```js
{
  name: 'Brand Name',
  desc: 'One line for the card.',
  logo: P + 'logos/brand-<slug>-logo.png',   // omit and add to noLogoBrands if none
  series: ['Wellness House'],                 // Catalyst | Wellness House | World of Sports
  blurb: 'Two-sentence background on what the brand is.',
  activation: 'What their moment on the floor actually looked like.',
  hero: 'assets/brands/<brand-slug>/1.jpg',
  images: ['assets/brands/<brand-slug>/1.jpg', 'assets/brands/<brand-slug>/2.jpg', 'assets/brands/<brand-slug>/3.jpg']
}
```

3. Add its revenue range to `revData`. Ordering and pagination handle themselves, 8 cards per page.

`P` is the asset host base (`https://willpowerhq.com/assets/`).

### Brand logos

Logos are pulled from the Willpower asset host and rendered white for uniformity. Upload new logos there as `logos/brand-<slug>-logo.png` rather than committing them here.

## Deployment

Hosted on GitHub Pages from the `main` branch, root folder. Any push to `main` redeploys automatically. No build step.

To publish for the first time: **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `/ (root)`**.

## Known issues

- Pricing shown is draft pending sign-off.
- Brands without a per-brand photo folder fall back to general event imagery in the gallery.
