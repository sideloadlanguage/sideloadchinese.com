# Implementation Plan: sideloadspanish.com → sideloadchinese.com

Source: `meowmeowco/sideloadspanish.com` (7 files, static site hosted on GitHub Pages)

---

## File-by-file changes

### CNAME

```
sideloadspanish.com → sideloadchinese.com
```

### index.html

**Title + meta**:
```html
<!-- Old -->
<title>Sideload Spanish — Learn Spanish While You Browse</title>
<meta name="description" content="A browser extension that replaces English words with Spanish on every page you read. Learn by doing, not by drilling.">
<link rel="canonical" href="https://sideloadspanish.com">

<!-- New -->
<title>Sideload Chinese — Learn Chinese While You Browse</title>
<meta name="description" content="A browser extension that replaces English words with Chinese on every page you read. Hanzi with pinyin, HSK-leveled. Learn by doing, not by drilling.">
<link rel="canonical" href="https://sideloadchinese.com">
```

**Logo**:
```html
<!-- Old -->
<a href="/" class="logo">sideload<span>spanish</span></a>

<!-- New -->
<a href="/" class="logo">sideload<span>chinese</span></a>
```

**Hero section — demo words**:

Replace inline Spanish demo words with Chinese ruby elements:

```html
<!-- Old -->
Learn <span class="sl-word" data-original="Spanish" data-es="español" data-tier="1">español</span>
while you browse

<!-- New -->
Learn <ruby class="sl-word" data-original="Chinese" data-zh="中文" data-pinyin="zhōngwén" data-hsk="1">中文<rp>(</rp><rt>zhōngwén</rt><rp>)</rp></ruby>
while you browse
```

All demo `.sl-word` spans in hero paragraph → convert to `<ruby>` elements:

| Original English | Old (Spanish) | New (Chinese) |
|-----------------|---------------|---------------|
| Spanish | español | 中文 (zhōngwén) |
| words | palabras | 词 (cí) |
| effort | esfuerzo | 努力 (nǔlì) |
| web | web | 网 (wǎng) |

**Install section**:
```html
<!-- Old -->
<p>Free forever. Works on Firefox today. Chrome pending review.</p>

<!-- New — adjust per actual store status -->
<p>Free forever. Chrome and Firefox coming soon.</p>
```

- Firefox AMO link → update to sideloadchinese addon URL (once submitted)
- Chrome Web Store link → update once available
- Both may start as "Coming soon" if not yet submitted

**Features section**:
```html
<!-- Old -->
<p>Start with the 500 most common words. As you learn them, harder words appear. Five tiers from A1 to C1.</p>

<!-- New -->
<p>Start with HSK 1 — the 500 most common words. As you learn them, harder characters appear. Six levels from HSK 1 to HSK 6.</p>
```

- "Five tiers from A1 to C1" → "Six levels from HSK 1 to HSK 6"
- All references to "Spanish" → "Chinese"
- "No flashcards" section stays as-is (concept identical)

**Pricing section**:
```html
<!-- Old -->
<li>5 tiers (A1 → C1)</li>

<!-- New -->
<li>6 levels (HSK 1 → HSK 6)</li>
```

- "5,000+ word vocabulary" → "5,000+ word HSK vocabulary"

**Waitlist section**:
```html
<!-- Old -->
action="https://buttondown.com/api/emails/embed-subscribe/sideloadspanish"

<!-- New -->
action="https://buttondown.com/api/emails/embed-subscribe/sideloadchinese"
```

Requires creating a `sideloadchinese` publication on Buttondown first.

**Tooltip script** (inline `<script>` at bottom):

Replace Spanish-specific tooltip with Chinese version:

```js
// Old
const TIER_LABELS = {
  1: 'A1 — Beginner',
  2: 'A2 — Elementary',
  3: 'B1 — Intermediate',
  4: 'B2 — Upper Intermediate',
  5: 'C1 — Advanced',
};

// New
const TIER_LABELS = {
  1: 'HSK 1 — Beginner',
  2: 'HSK 2 — Elementary',
  3: 'HSK 3 — Intermediate',
  4: 'HSK 4 — Upper Intermediate',
  5: 'HSK 5 — Advanced',
  6: 'HSK 6 — Mastery',
};
```

Tooltip `show()` function:
```js
// Old — reads data-es, data-gender
const es = target.dataset.es;
const gender = target.dataset.gender;
transEl.textContent = '→ ' + es;

// New — reads data-zh, data-pinyin
const zh = target.dataset.zh;
const pinyin = target.dataset.pinyin;
transEl.textContent = '→ ' + zh + ' (' + pinyin + ')';
// Remove gender block entirely
```

**Footer**:
```html
<!-- Old -->
<a href="https://github.com/meowmeowco/sideloadspanish">Source</a>

<!-- New -->
<a href="https://github.com/sideloadlanguage/sideloadchinese">Source</a>
```

### styles.css

**CJK font** — add to `:root` or body:
```css
body {
  font-family: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", system-ui, -apple-system, sans-serif;
}
```

**Colour scheme** — consider changing accent from Spanish orange to Chinese red:
```css
:root {
  --orange: #c0392b;       /* Chinese red */
  --orange-dark: #a93226;
}
```

This is a branding decision — can keep orange if preferred.

**Ruby text in demo words**:
```css
ruby.sl-word rt {
  font-size: 0.6em;
  opacity: 0.85;
}
```

**Tooltip gender class** — remove `.sl-tooltip__gender` (not needed for Chinese).

### privacy.html

Global find-replace:
- "Sideload Spanish" → "Sideload Chinese"
- `meowmeowco/sideloadspanish` → `sideloadlanguage/sideloadchinese`
- "← Back to Sideload Spanish" → "← Back to Sideload Chinese"

### sync-success.html

- "Sideload Spanish" → "Sideload Chinese" in title and CTA link text

### _headers

No changes needed. CSP policy is generic.

### .nojekyll

No changes needed.

---

## New assets needed (not in current repo)

None currently — site has no images or screenshots in the repo (those are in the extension repo under `sideload/store/`). If screenshots are added later, they should show Chinese replacements.

---

## Pre-deployment checklist

- [ ] DNS: point `sideloadchinese.com` to GitHub Pages
- [ ] Buttondown: create `sideloadchinese` publication
- [ ] GitHub Pages: enable on `sideloadlanguage/sideloadchinese.com` repo
- [ ] Store links: update once extension is submitted to Chrome/Firefox
- [ ] Test: verify tooltip demo works with ruby elements on mobile + desktop
