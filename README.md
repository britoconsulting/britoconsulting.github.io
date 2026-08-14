# Brito Consulting Services LLC — Website

Single-file marketing site for an independent SAP ILM (Information Lifecycle
Management) and data archiving practice.

- **Live URL:** https://britoconsulting.github.io
- **Stack:** one `index.html`. No build step, no npm, no framework.
- **External dependencies:** Google Fonts (Archivo, Inter, JetBrains Mono — one
  request) and Chart.js **4.5.0** from cdnjs. Nothing else.
- **Page weight:** 317 KB excluding fonts, 497 KB transferred in total across
  6 requests. Time to interactive on a throttled 4G profile is 0.62 s.
- **Accessibility:** every text/background pairing is measured programmatically
  at the brightest point of the compositing stack; the worst pairing on the page
  is 5.78:1 for normal text and 4.54:1 for large. WCAG AA throughout.
  `prefers-reduced-motion` disables every transform and scroll effect.
- No analytics, no cookies, no third-party trackers.

## Local preview

Double-clicking the file works, but serve it over HTTP for an exact match to
production:

```
python -m http.server 8000     # or:  npx --yes http-server -p 8000
```

Then open http://localhost:8000. `http-server` caches for an hour — append
`?v=2` (incrementing) to force a fresh load after each edit.

## Deploying

The repository is served by GitHub Pages from the `main` branch, `/ (root)`.
Edit `index.html`, commit, push — Pages redeploys automatically, usually within
a minute. If the change does not appear, hard-refresh (Ctrl+Shift+R /
Cmd+Shift+R).

## Structure

Everything is in `index.html`:

| Where | What |
|---|---|
| `<head>` | Title, meta description, Open Graph / Twitter cards, canonical, inline SVG favicon, fonts |
| `<style>` → `:root` | Every colour, type size, spacing step, radius, shadow and transition. No hardcoded colours exist below this block. |
| `<style>` → rest | Base, components, breakpoints at 640 / 768 / 1024 / 1280, `prefers-reduced-motion` |
| `<body>` | 14 sections, each wrapped in `<!-- ==== SECTION: NAME — START/END ==== -->` markers |
| `<script>` → `CONFIG` | All page content as plain data — edit content here, not in the markup |
| `<script>` → below `CONFIG` | Rendering, calculator, nav, FAQ, form, motion |

## Contact form

The form posts to Web3Forms via `fetch()` and renders success and error states
inline — it never redirects off-site. The access key lives in
`CONFIG.web3formsKey`; the hidden `access_key` input in the CONTACT section is a
no-JS fallback that `CONFIG` overwrites on load. To rotate the key, sign in at
web3forms.com and replace the value in `CONFIG`.

The destination address is bound to the key on Web3Forms' side, so it appears
nowhere in this repository or in the rendered page.

---

© Brito Consulting Services LLC. Site content and copy are not licensed for reuse.
