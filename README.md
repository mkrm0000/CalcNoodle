# CalcNoodle 🍜

> An open-source collection of engineering & study tools, built by students, for students — because your calculator shouldn't cost a semester's tuition.

**CalcNoodle** is a community-built hub of small, focused tools for engineering students and anyone who needs to crunch numbers, convert units, or look up a formula without wading through ads and sign-up walls.

🌐 **Live site:** https://mkrm0000.github.io/calcnoodle/
💻 **Repo:** https://github.com/mkrm0000/calcnoodle

---

## Status

🚧 **Early development.** The structure, styling system, and first tools are being built. Things may change. Contributions are very welcome — see [`CONTRIBUTING.md`](./CONTRIBUTING.md).

---

## What's inside (planned)

Tools are grouped by category:

| Category | Examples |
|---|---|
| **Electrical** | Resistor color codes, Ohm's law, power calculators |
| **Mechanical** | Beam deflection, stress/strain, torque |
| **Civil** | Section modulus, depth finder, load calculations |
| **Math** | Matrix operations, equation solvers |
| **Study** | GPA calculator, unit conversions, Morse code |
| **General** | Knots → m/s, temperature, length, and other converters |

### First tools in the pipeline

- 🔤 **Morse code** — encode/decode text ↔ Morse (General)
- 🏗️ **Section modulus calculator** — for beam cross-sections (Civil)
- ⚓ **Knots → m/s** — nautical speed converter (General)
- 🌊 **Depth finder** — sonar/travel-time depth estimation (Civil)

---

## Why another tools site?

- **No accounts. No paywalls.** Everything runs in your browser.
- **Open source.** Fork it, fix it, host it yourself.
- **Student-friendly.** Built by students who actually need these tools.
- **Ads are optional.** CalcNoodle works 100% with ads disabled. When ads are shown, they never block a tool.
- **Small and fast.** Static HTML + vanilla JS. No framework, no build step.

---

## Tech stack

- **Pure HTML, CSS, and vanilla JavaScript** — no frameworks, no build step
- **Hosted on GitHub Pages**
- **Global header, footer, and ad system** injected by a shared script so every tool looks and feels the same
- **Naming conventions and design tokens** documented in [`CONTRIBUTING.md`](./CONTRIBUTING.md) and `docs/STYLE-GUIDE.md` (coming soon)

---



> **Path rule (important):** GitHub Pages serves this repo at `https://mkrm0000.github.io/calcnoodle/`, so **root-relative paths (`/assets/...`) will break in production**. Every page uses **relative paths**:
> - Homepage: `./assets/global-style.css`
> - Tool pages in `tools/<slug>/`: `../../assets/global-style.css`
>
> The tool template enforces this — just copy it.

---

## Running locally

Because the site fetches `header.html` and `footer.html` at runtime, opening `index.html` via `file://` won't work (browsers block `fetch` on `file://`). Serve it over HTTP instead:

```bash
# From the repo root — pick one:
python -m http.server 8000
# or
npx serve .
# or
php -S localhost:8000
```

Then open http://localhost:8000/.
