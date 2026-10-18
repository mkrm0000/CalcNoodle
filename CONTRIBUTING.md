# Contributing to CalcNoodle

Thanks for wanting to help! CalcNoodle is built by students, for students — anyone can contribute, and first-time open-source contributors are especially welcome.

This document covers:

1. [Code of Conduct](#code-of-conduct)
2. [What to work on](#what-to-work-on)
3. [Local development](#local-development)
4. [Project layout](#project-layout)
5. [Naming conventions](#naming-conventions)
6. [Adding a new tool](#adding-a-new-tool)
7. [Ads system](#ads-system)
8. [Path rules](#path-rules)
9. [Commit and PR rules](#commit-and-pr-rules)
10. [Licensing](#licensing)

---

## Code of Conduct

By participating, you agree to follow our [Code of Conduct](./CODE_OF_CONDUCT.md). Report violations by opening an issue with the subject line **`CoC Violation`**.

---

## What to work on

Good first contributions:

- 🐛 Fix a bug in an existing tool
- 🎨 Improve the global styles (`assets/global-style.css`)
- 📝 Improve docs
- 🧪 Add a new tool (see below — this is the fun one)

Before starting anything big, **open an issue first** so we can agree on the approach and avoid duplicated work.

---

## Local development

CalcNoodle is a static site — no build step. But because the header and footer are fetched at runtime, you **must serve it over HTTP** (opening `index.html` via `file://` won't work).

```bash
# From the repo root — pick one:
python -m http.server 8000
# or
npx serve .
# or
php -S localhost:8000
```

Open <http://localhost:8000/>.

When you add a tool, make sure it also works when served from a *subpath* (as it will be on GitHub Pages at `/calcnoodle/`). The **relative-path rule** below handles this.

---

## Project layout

```
calcnoodle/
├── index.html
├── 404.html
├── tools.json
├── assets/
│   ├── global-style.css
│   ├── global-script.js
│   ├── ads.js
│   ├── header.html
│   ├── footer.html
│   └── logo.svg
├── tools/
│   └── <tool-slug>/
│       ├── index.html
│       ├── style.css     ← optional, tool-specific only
│       └── script.js     ← optional
├── templates/
│   └── tool-template/
└── docs/
```

---

## Naming conventions

CalcNoodle uses a small, strict set of conventions. Follow them — PRs that don't will be asked to change.

### CSS — `mt-` prefix, BEM-style

All global classes start with `mt-` (short for "Maritools", the project's former name — kept for backwards compatibility and clarity).

| Purpose | Example |
|---|---|
| Layout wrapper | `.mt-container` |
| Header | `.mt-header`, `.mt-header__logo`, `.mt-header__nav` |
| Footer | `.mt-footer`, `.mt-footer__links` |
| Buttons | `.mt-btn`, `.mt-btn--primary`, `.mt-btn--ghost` |
| Cards | `.mt-card`, `.mt-card__title` |
| Forms | `.mt-input`, `.mt-label`, `.mt-form-group` |
| Utilities | `.mt-text-center`, `.mt-mt-2`, `.mt-hidden` |

**Your tool's own classes:** prefix with your tool's slug to avoid collisions, e.g. `.sectionmod-result`, `.morse-output`.

### IDs

IDs are for **JavaScript hooks**, not for styling. Every global ID uses the `mt-` prefix.

- Reserved IDs (don't use for anything else): `mt-header`, `mt-footer`
- Ad slots (fixed): `ad-small`, `ad-big`, `ad-wide`, `ad-poster`
- Your tool: use slugs, e.g. `sectionmod-input`

### CSS custom properties

Design tokens live in `:root` of `assets/global-style.css`. Never hardcode colors, radii, or spacing — always use the token:

```
--mt-color-primary
--mt-color-accent
--mt-color-bg
--mt-color-text
--mt-font-body
--mt-font-heading
--mt-radius
--mt-space-1 … --mt-space-8
--mt-header-height
--mt-footer-height
```

### JavaScript

- Vanilla JS only — no frameworks, no bundlers
- 2-space indentation, single quotes, semicolons
- Prefer `const` and `let`; no `var`
- Prefix any global you attach to `window` with `mt` (e.g. `window.mtOpenTool`)

### HTML

- 2-space indentation, double-quoted attributes
- `lang="en"` on every page
- Always include `<meta charset="utf-8">` and viewport meta
- Use semantic tags (`<main>`, `<header>`, `<footer>`, `<nav>`) where they apply

### Files

- Tool folder names: **kebab-case** (`section-modulus`, `morse-code`, `knots-to-ms`)
- No spaces, no uppercase in filenames

---

## Adding a new tool

1. **Copy the template:**

   ```bash
   cp -r templates/tool-template tools/your-tool-slug
   ```

2. **Edit `tools/your-tool-slug/index.html`.** The template already:
   - Links `global-style.css` with the correct `../../` depth
   - Includes `<div id="mt-header"></div>` and `<div id="mt-footer"></div>`
   - Includes the two global scripts (`global-script.js`, `ads.js`)
   - Wraps your UI in `<main class="mt-container">`

   Set the `<title>` to `Your Tool Name — CalcNoodle`.

3. **Register the tool in `tools.json`:**

   ```json
   {
     "slug": "your-tool-slug",
     "name": "Your Tool Name",
     "description": "One sentence about what it does.",
     "category": "General",
     "path": "tools/your-tool-slug/"
   }
   ```

   Valid categories: `Electrical`, `Mechanical`, `Civil`, `Math`, `Study`, `General`.

4. **Optional:** add `style.css` and `script.js` inside your tool folder. Only style your own tool — don't edit `global-style.css` unless you're changing something genuinely global.

5. **Test locally** with `python -m http.server 8000` and click through from the homepage.

6. **Open a pull request.** Fill out the PR template.

---

## Ads system

Ads are **optional** — a tool must work perfectly with `ads.js` removed. The ad system is documented in detail in `docs/ADS.md` (coming soon). The short version:

- Four fixed slot IDs exist: `ad-small`, `ad-big`, `ad-wide`, `ad-poster`
- To request an ad, put **one** `<div id="ad-…"></div>` in your page
- Don't style `#ad-*` — the global CSS reserves the space
- Never put an ad between a label and its input, or in the middle of an interactive flow

---

## Path rules

The site is served from `https://mkrm0000.github.io/calcnoodle/` — a **subpath**. Root-relative paths (`/assets/...`) will break in production.

**Always use relative paths.** The exact depth depends on where the page lives:

| Page location | Path to `assets/` |
|---|---|
| `index.html` (root) | `./assets/global-style.css` |
| `tools/<slug>/index.html` | `../../assets/global-style.css` |
| `404.html` (root) | `./assets/global-style.css` |

If you copy the tool template, this is already correct — don't change the depth.

**Never** hardcode `https://mkrm0000.github.io/calcnoodle/...` in a page — always use relative paths so contributors can test locally.

---

## Commit and PR rules

### Commits

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add morse code encoder
fix: correct section modulus formula for T-beams
docs: clarify path rules in CONTRIBUTING
chore: add prettier config
style: format global-style.css
refactor: extract ad rendering helper
```

### Pull requests

- **One logical change per PR.** "Add morse code tool" is one PR; "Add morse code tool and fix footer" is two.
- **Branch from `main`.** Short-lived branches. No `dev` branch.
- **Fill out the PR template** when it lands (checklist included).
- **Link the issue** your PR closes: `Closes #12`.
- **Screenshots or GIFs** for any UI change.
- **Don't touch `global-style.css` / `global-script.js` / `ads.js`** in a tool PR — if you need a change there, open a separate PR and explain why.

### Reviews

- Be patient — this is a student project maintained in spare time.
- Be kind. See the Code of Conduct.

---

## Licensing

CalcNoodle is MIT-licensed. By submitting a pull request, you agree that your contributions are licensed under the terms in [`LICENSE`](./LICENSE).

No DCO sign-off is required — opening a PR is your agreement.
