# Fika site — deploy (free, no domain)

Static single page. No build step, no framework, no dependencies.
GitHub holds the code, Cloudflare Pages hosts it and redeploys on every push.

---

## Before you deploy — 3 edits

Open `index.html`, scroll to the `<script>` at the very bottom. Everything you need is in the first
20 lines:

```js
const WHATSAPP_NUMBER  = '254700000000';   // ← your WhatsApp Business number, digits only
const INSTAGRAM_HANDLE = 'fika.events';    // ← your IG handle, no @
const NIGHTS = [ ... ];                    // ← the 12 nights: date, title, price, one-liner
```

That's it. Every "Book" button on the page builds its own WhatsApp link, pre-filled with the name and
date of the next upcoming night — so the message you receive already tells you which Tuesday they want.
Past dates grey out on their own; "Up next" moves along by itself. **You never have to edit the page
week to week.**

## The logo — done

`assets/logo.png` is your file, trimmed of its transparent padding and scaled to 1100px wide (86KB).
Two recoloured versions are generated from it and already wired in:

- `logo-ink.png` — **#220D0C**, used in the header on the light background
- `logo-sand.png` — **#D6C8BD**, used in the footer on the dark background

Your original artwork is exactly `#D6C8BD`, so `logo-sand.png` is identical to the source in colour.
If you ever replace the artwork, drop the new file in as `logo.png` and ask me to regenerate the two.

## The fonts

**Headers: Adigiana Toybox.** The site is already wired for it — the `@font-face` at the top of
`index.html` points at `assets/fonts/AdigianaToybox.woff2` (falling back to `.ttf`). Drop either file in
that folder and every heading switches over with no other change. Until then the stack falls through to
**Baloo 2**, which is close in weight and roundness, so nothing looks broken in the meantime.

> ⚠️ **Licence:** Adigiana Toybox is free for *personal* use — commercial use needs permission from the
> designer (Daniel Lyons Visual Design). Fika sells tickets, so that's a commercial use. Worth an email
> before you print anything or run ads with it.

**Body: Figtree** (Google Fonts, free for commercial use, already loading). It's a warm humanist sans
with slightly rounded terminals, so it sits naturally beside a bubbly display face without competing
with it — and it stays sharp at 15px, which the display font wouldn't.

**Accents: Caveat** for the handwritten lines ("the run of the night", "what it costs"), matching the
`events` mark under the logo.

## The palette

Your four brand colours, used exactly:

| | Hex | Where |
|---|---|---|
| Ink | `#220D0C` | body text, dark sections, footer, header logo |
| Oxblood | `#541409` | the accent — headline, CTAs, links, active states |
| Taupe | `#9B8174` | decorative only (drifting names, highlight underline, glow) |
| Sand | `#D6C8BD` | borders, dividers, timeline dots, footer logo |

Page and card backgrounds are the **sand composited on white** (~40% and ~18%) — tints of your colour,
not a fifth hue. Taupe is deliberately never used for small text: against the page background it lands
around 2.7:1 contrast, which is below the readable threshold, so body copy uses ink at reduced opacity
instead.

---

## 1. Put it on GitHub

From this folder (`Fika Events/website`):

```bash
git init -b main && git add -A && git commit -m "Fika site"
```

Then create the repo and push (GitHub CLI):

```bash
gh repo create fika-events --public --source=. --push
```

No `gh`? Create an empty repo called `fika-events` on github.com, then:

```bash
git remote add origin https://github.com/YOUR-USERNAME/fika-events.git && git push -u origin main
```

## 2. Host it on Cloudflare Pages

1. dash.cloudflare.com → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. Authorise GitHub, pick **fika-events**
3. Build settings — leave everything empty:
   - Framework preset: **None**
   - Build command: *(blank)*
   - Build output directory: **`/`**
4. **Save and Deploy**

Live in about a minute at **`https://fika-events.pages.dev`**.

Every `git push` from then on redeploys automatically.

## 3. Later, when you buy a domain

Cloudflare Pages → your project → **Custom domains** → **Set up a domain**. If the domain is already on
Cloudflare, DNS is filled in for you. Nothing in the site needs changing.

---

## Notes

- Fonts (Baloo 2, Caveat, Karla) and Tailwind load from CDNs — the page needs a connection, which is
  fine for a marketing site.
- Once the real brand fonts are confirmed from the Canva file, swap the `<link>` in `<head>` and the
  `fontFamily` block in the Tailwind config near the top.
- `og:` tags are set, so the link preview looks right when shared to WhatsApp and IG.
