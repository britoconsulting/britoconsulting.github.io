# Brito Consulting Services LLC — Website

Single-file marketing site for an independent SAP ILM (Information Lifecycle
Management) and data archiving practice.

- **Live URL:** https://britoconsulting.github.io
- **Repo:** `britoconsulting/britoconsulting.github.io`
- **Stack:** one `index.html`. No build step, no npm, no framework.
- **External dependencies:** Google Fonts (Inter) and Chart.js **4.5.0** from cdnjs. Nothing else.

**Previewing changes locally.** Double-clicking the file works, but serve it over HTTP
if you want an exact match to production:

```powershell
cd "C:\Users\neeha\OneDrive\Documents\COWORK\Brito Consulting Website"
python -m http.server 8000     # or:  npx --yes http-server -p 8000
```

Then open http://localhost:8000. Note `http-server` caches for an hour — append
`?v=2` (incrementing) to the URL to force a fresh load after each edit.

---

## 1. Deploy to GitHub Pages

**First-time setup**

1. Create a GitHub repository named exactly `britoconsulting.github.io` under the
   `britoconsulting` account. The name must match the account name — that is what
   makes it a user site served at the root domain.
2. Make it **Public**. GitHub Pages will not serve a private repo on the free tier.
3. Upload `index.html` and `README.md` to the repository root
   (**Add file → Upload files**, or `git push` — see below).
4. Go to **Settings → Pages**.
   - **Source:** Deploy from a branch
   - **Branch:** `main`, folder `/ (root)`
   - **Save**
5. Wait 1–2 minutes. The site is live at https://britoconsulting.github.io

**From the command line**

```bash
git clone https://github.com/britoconsulting/britoconsulting.github.io.git
cd britoconsulting.github.io
# copy index.html and README.md into this folder
git add .
git commit -m "Publish site"
git push origin main
```

**Publishing an update**

Edit `index.html`, commit, push. GitHub Pages redeploys automatically, usually
within a minute. If you do not see the change, hard-refresh (Ctrl+Shift+R /
Cmd+Shift+R) — the browser caches aggressively.

---

## 2. Contact form setup (Web3Forms)

The contact form posts to Web3Forms via `fetch()`. It never redirects off-site;
success and error states render inline.

**The destination email address is deliberately not stored in this file.**
Web3Forms binds it to the access key on their side, which keeps the address out of
the page source and away from scrapers.

1. Go to https://web3forms.com
2. Enter the destination email address. They email you an access key (a UUID).
3. Open `index.html`, find `web3formsKey` near the top of the `CONFIG` object, and
   replace the placeholder:

   ```js
   web3formsKey: 'YOUR_WEB3FORMS_ACCESS_KEY_HERE',
   ```

   with

   ```js
   web3formsKey: 'paste-your-key-here',
   ```

4. Commit and push. Submit a test enquiry through the live site and confirm the
   email arrives.

Notes:
- The hidden `access_key` input in the CONTACT section is overwritten from `CONFIG`
  on page load, so `CONFIG.web3formsKey` is the only value you need to change.
- A hidden `botcheck` honeypot field is already wired in for spam filtering.
- The free tier covers 250 submissions/month, which is well beyond what this site
  will generate.

---

## 3. Palette and background art

**Palette — "Ink & Electric Blue."** All five values are CSS custom properties in
`:root`. There are no hardcoded colours anywhere below that block, so a rebrand is
a `:root` edit and nothing else.

| Token | Value | Role |
|---|---|---|
| `--c-navy` | `#0A1020` | base, navy-black |
| `--c-navy-elev` | `#14213D` | elevated surfaces, cards |
| `--c-blue` | `#3B82F6` | electric blue — fills, glows, chart, borders |
| `--c-blue-text` | `#7DB3FB` | lighter tint — small text only |
| `--c-gold` | `#FBBF24` | amber — CTAs and key figures |
| `--c-text` | `#F8FAFC` | body text |
| `--c-muted` | `#A3B0C2` | secondary text |

**Two accents, deliberately split.** Amber carries primary emphasis (CTAs, big
numbers, outcome lines). Electric blue carries secondary emphasis (eyebrows,
section numerals, icons, borders, glow fields). Keeping that split is what stops
the page looking like a Christmas tree.

**One accessibility rule.** `--c-blue` (`#3B82F6`) measures 4.3:1 against
`--c-navy-elev` — below the 4.5:1 WCAG AA threshold for normal text. Use it for
large text, icons, borders and fills only. For small text use `--c-blue-text`
(`#7DB3FB`), which measures 7.4:1. Contrast for every pairing is tabulated in a
comment above the palette in `:root`.

The gradient fields lighten the surface under text, so `--c-blue-text` and
`--c-muted` are set one step lighter than the raw Tailwind values. That keeps
them at 5.1:1 or better even at the brightest point of a field, where the naive
values fell to 4.4:1.

**Backgrounds are generated, not photographed.** Each section gets a soft
gradient "field" (`--field-hero`, `--field-cool`, `--field-warm`, `--field-focus`,
`--field-convert`) so the page changes colour temperature top to bottom instead of
being one flat navy. The hero also carries an SVG "data strata" motif — bars
compressing as they descend, echoing the logo and the archiving story. No image
files, no licensing, no extra requests.

### Adding a photo later

`CONFIG.images` has two slots, both empty by default:

```js
images: {
  hero: '',      // e.g. 'images/hero.jpg' or a full https:// URL
  contact: ''
}
```

Fill either one and a photo layer appears beneath a dark scrim that keeps text
legible and the palette intact. Leave them empty and only the gradient fields show.

**Choosing the image matters more than whether you use one.** Good: abstract
long-exposure light trails, fibre-optic strands, dark server-aisle geometry,
architectural lattice, macro circuitry — dark, cool, no focal subject. Avoid:
people shaking hands, glowing "digital brain" composites, and cityscape-at-dusk,
which is the exact shot the incumbent SAP consultancies all use.

Free and commercially usable, no attribution required: **Unsplash**
(unsplash.com), **Pexels** (pexels.com), **Pixabay** (pixabay.com). Prefer
downloading into an `images/` folder and committing it over hotlinking, so the
page does not break when a CDN URL changes.

---

## 4. How to update this site with Claude

Open the folder in Claude and paste one of the prompts below. The site is built so
that content lives as plain data in a single `CONFIG` object at the top of the
script — Claude can change a line of data without touching layout or CSS.

**Prompt 1 — add a case result**

```
In index.html, add a new entry to CONFIG.caseResults for a global energy utility:
situation "Meter and billing data growth degrading month-end reporting", metric
"~30%", metricLabel "reduction in reporting table volume", detail "Archiving
objects deployed across FI-CA and IS-U with Archive Information Structures for
business read-access." Keep the client anonymous — no company name. Change
nothing else in the file.
```

**Prompt 2 — add an FAQ item**

```
In index.html, add one entry to CONFIG.faq: question "Can archiving run while the
business is live?" with a technically accurate answer of 60–90 words covering
job scheduling in low-activity windows, variant-based selection, and the fact
that write and delete phases are separated. Match the existing tone — no
marketing language. Change nothing else.
```

**Prompt 3 — change the palette**

```
In index.html, change the accent colour from gold to a deep copper across the
whole site. Edit only the :root design token block — do not touch any other part
of the file. Keep WCAG AA contrast against the navy background and tell me the
contrast ratio you landed on.
```

**Other things worth asking for:** update a credibility-bar figure
(`CONFIG.stats`), reword the hero (`CONFIG.heroSub` plus the `<h1>` in the HERO
section), adjust calculator defaults or slider bounds (`CONFIG.calculator`), or
add an industry to the strip (`CONFIG.industries`).

---

## 5. File map for whoever edits this next

Everything is in `index.html`, in this order:

| Where | What |
|---|---|
| `<head>` | Title, meta description, Open Graph / Twitter cards, canonical, inline SVG favicon, Google Fonts |
| `<!-- MAINTENANCE NOTES -->` | Orientation block — read it first |
| `<style>` → `:root` | Every colour, type size, spacing step, radius, shadow and transition. No hardcoded colours exist below this block. |
| `<style>` → rest | Base, components, then breakpoints at 640 / 768 / 1024 / 1280, then `prefers-reduced-motion` |
| `<body>` | 14 sections, each wrapped in `<!-- ==== SECTION: NAME — START/END ==== -->` markers |
| `<script>` → `CONFIG` | All business content as plain data |
| `<script>` → below CONFIG | Rendering, calculator engine, nav, FAQ, form, motion |

**Section marker names** (grep any of these to jump to a section):
NAV · HERO · CREDIBILITY BAR · INDUSTRIES STRIP · THE COST OF DOING NOTHING ·
SERVICES · CALCULATOR · APPROACH · CASE RESULTS · FEDERAL CAPABILITY · ABOUT ·
FAQ · CONTACT · FOOTER

**Calculator math** lives in `computeCalc()` under the `CALCULATOR ENGINE` banner:

```
unarchived[y]  = size * (1 + growth)^y
archived[y]    = size * (1 - archivable) * (1 + growth)^y
costAvoided    = SUM(y=1..5) (unarchived[y] - archived[y]) * costPerTB
tbRemovedYear1 = size * archivable
```

Worked check with the shipped defaults (6 TB, 18% growth, 40% archivable,
$4,000/TB): 5-year size 13.7 TB unarchived vs 8.2 TB archived, $81,043 cumulative
cost avoided, 2.4 TB removed in year 1.

---

## 6. Content rules baked into this site

These were deliberate decisions. Preserve them unless the business changes.

- No pricing published anywhere.
- No client names, employer names, agency names, or program names. Anonymized
  descriptors only.
- No headshot, no phone number, no booking link.
- No stock photography.
- No cookie banner, no analytics, no third-party trackers.
- No urgency or scarcity language.
- The destination email address appears nowhere in the rendered page or source.
- Every figure on the page traces to a delivered engagement outcome. The
  "cost of doing nothing" section is deliberately qualitative — no invented
  industry statistics.
