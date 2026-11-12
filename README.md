# Glitch Lab — Geometric Image Processor

<p align="center">
  <a href="https://ayushshukla73.github.io/digital-geometric-glitch-art/"><strong>Live Demo</strong></a> •
  <a href="#transformation-pipeline">Pipeline</a> •
  <a href="#usage">Usage</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#local-development">Local Development</a> •
  <a href="#screenshots">Screenshots</a> •
  <a href="#accessibility">Accessibility</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#license">License</a>
</p>

---

A zero-dependency, browser-based geometric glitch art generator that transforms uploaded images through a 4-step pipeline:

1. **2×2 Mirror Grid** — Creates a four-quadrant composite (original, H-flip, V-flip, both)
2. **Vertical Strip Recombination** — Slices into 40px columns, recombines alternating from left/right edges inward
3. **Horizontal Strip Recombination** — Slices into 40px rows, recombines alternating from top/bottom edges inward
4. **Export** — Downloads the final result as PNG

Built with vanilla JavaScript and the HTML5 Canvas API. No build step, no dependencies, runs entirely in the browser.

---

## Transformation Pipeline

| Step | Operation | Parameters | Description |
|------|-----------|------------|-------------|
| **1** | 2×2 Mirror Grid | — | Composites original image in 4 orientations: normal (TL), horizontal flip (TR), vertical flip (BL), both flips (BR) |
| **2** | Vertical Strip Split | 40px strip width | Splits the 2×2 grid into vertical strips, draws guide lines |
| **2b** | Vertical Recombination | Alternating L→R | Reassembles strips by taking one from left edge, then right edge, then left+1, then right-1, continuing inward |
| **3** | Horizontal Strip Split | 40px strip height | Splits the vertical result into horizontal strips, draws guide lines |
| **3b** | Horizontal Recombination | Alternating T→B | Reassembles strips by taking one from top edge, then bottom edge, then top+1, then bottom-1, continuing inward |
| **4** | Final Export | PNG | Renders final canvas and enables download |

**Algorithm Details**

The strip recombination uses a two-pointer approach working from edges toward center:

```javascript
// Vertical (Step 2)
let li = 0, ri = nStrips - 1, dx = 0, toggle = true;
while (li <= ri) {
  const sx = toggle ? li * stripW : ri * stripW;
  const sw = Math.min(stripW, width - sx);
  ctx.drawImage(source, sx, 0, sw, height, dx, 0, sw, height);
  dx += sw;
  toggle ? li++ : ri--;
  toggle = !toggle;
}

// Horizontal (Step 3) — same pattern, transposed
let ti = 0, bi = nStrips - 1, dy = 0, toggle = true;
while (ti <= bi) {
  const sy = toggle ? ti * stripH : bi * stripH;
  const sh = Math.min(stripH, height - sy);
  ctx.drawImage(source, 0, sy, width, sh, 0, dy, width, sh);
  dy += sh;
  toggle ? ti++ : bi--;
  toggle = !toggle;
}
```

---

## Usage

1. **Upload** — Click the upload area or drag & drop an image (PNG, JPG, WebP, GIF ≤ 50 MB)
2. **Process** — Watch all four pipeline stages render in real time with progress indicator
3. **Inspect** — Switch between tabs to view intermediate results at each stage
4. **Download** — Click **Download PNG** to save the final glitch art

**Keyboard Shortcuts**
- `Enter` / `Space` on upload card — Open file picker
- `Tab` / `Shift+Tab` — Navigate between tabs and controls
- `←` / `→` — Switch tabs when tab bar is focused

---

## Architecture

### Technology Stack
- **HTML5** — Semantic markup, drag-and-drop API, FileReader API
- **CSS3** — Custom properties (CSS variables), Grid/Flexbox, `@keyframes` animations, `prefers-color-scheme`, `prefers-reduced-motion`
- **Vanilla JavaScript (ES2020+)** — Canvas 2D API, zero external dependencies
- **Google Fonts** — Syne (display) + JetBrains Mono (UI/mono)

### Project Structure
```
.
├── index.html              # Complete application (HTML + CSS + JS)
├── screenshots/            # Pipeline step screenshots for documentation
├── test-image.png          # Sample image for quick testing
├── .github/
│   ├── repository_description.txt   # GitHub repo description (≤350 chars)
│   └── repository_topics.txt        # GitHub topics/tags
├── LICENSE                 # MIT License
├── CONTRIBUTING.md         # Contribution guidelines
├── CODE_OF_CONDUCT.md      # Contributor Covenant 2.1
├── SECURITY.md             # Security policy
├── CHANGELOG.md            # Version history
└── .gitignore              # Git ignore rules
```

### Key Implementation Details

**Canvas Mirror Transformations (Step 1)**
```javascript
// Top-right: horizontal flip
ctx.save(); ctx.translate(W * 2, 0); ctx.scale(-1, 1); ctx.drawImage(img, 0, 0); ctx.restore();

// Bottom-left: vertical flip
ctx.save(); ctx.translate(0, H * 2); ctx.scale(1, -1); ctx.drawImage(img, 0, 0); ctx.restore();

// Bottom-right: both flips
ctx.save(); ctx.translate(W * 2, H * 2); ctx.scale(-1, -1); ctx.drawImage(img, 0, 0); ctx.restore();
```

**Progressive Rendering** — Each pipeline step updates the UI incrementally with a loading overlay, progress bar, and pipeline step indicators.

**Responsive Design** — Breakpoints at 860px and 480px; canvas grid uses `auto-fit` with `minmax(260px, 1fr)`.

---

## Local Development

No build tools required. Serve the directory with any static file server:

```bash
# Using npx (Node.js)
npx http-server . -p 8080 -c-1

# Using Python
python -m http.server 8080

# Using PHP
php -S localhost:8080
```

Then open `http://localhost:8080`.

**Requirements**
- Modern browser with Canvas 2D API support (Chrome 60+, Firefox 55+, Safari 11+, Edge 79+)
- No Node.js, npm, or build step required for runtime

---

## Screenshots

### Pipeline Visualization

| Stage | Preview | Description |
|-------|---------|-------------|
| **Original** | ![Initial Image](screenshots/initial%20image.png) | Source image before processing |
| **Step 1: 2×2 Mirror Grid** | ![Complete 2×2 Grid](screenshots/Complete%202%C3%972%20Grid.png) | Four quadrants: normal, H-flip, V-flip, both |
| **Step 2: Vertical Strips — Split** | ![Split Vertical](screenshots/Split%20into%2040px%20vertical%20strips.png) | 40px vertical strips with guide lines |
| **Step 2b: Vertical Recombined** | ![Recombined Vertical](screenshots/Recombined%20(left,%20right,%20left%2B1,%20right-1...).png) | Strips recombined L→R alternating |
| **Step 2c: Vertical Joined** | ![Joined Vertical](screenshots/Joined%20result.png) | Final vertical recombination result |
| **Step 3: Horizontal Strips — Split** | ![Split Horizontal](screenshots/Split%20into%20horizontal%20strips.png) | 40px horizontal strips with guide lines |
| **Step 3c: Horizontal Joined** | ![Joined Horizontal](screenshots/Joined%20result.png) | Final horizontal recombination result |
| **Step 4: Final Output** | ![Final Processed](screenshots/final%20processed%20image.png) | Complete glitch art after all transforms |

> **Note:** All screenshots are from the live demo processing the sample `test-image.png`.

---

## Accessibility

Glitch Lab is designed to be usable by everyone:

- **Keyboard Navigation** — All interactive elements reachable and operable via keyboard (`tabindex`, `role="button"`, `Enter`/`Space` activation)
- **ARIA** — Live regions (`aria-live="polite"` for results, `aria-live="assertive"` for loader), `aria-label`/`aria-describedby` on controls, `role="tablist"`/`role="tab"`/`role="tabpanel"` for tabbed interface
- **Focus Management** — Visible focus rings (`:focus-visible`), logical tab order, focus trapped in loader overlay
- **Reduced Motion** — `@media (prefers-reduced-motion: reduce)` disables all animations/transitions
- **Color Contrast** — WCAG AA compliant in both light and dark modes (custom properties swap on `prefers-color-scheme`)
- **Semantic HTML** — Proper heading hierarchy (`h1` → `h2`), landmarks (`header`, `main` implicit, `footer`), form labels

---

## Performance

- **Zero Dependencies** — ~36 KB total (HTML + CSS + JS), no external JS bundles
- **Canvas Operations** — All processing on `OffscreenCanvas`-compatible 2D context; strip recombination is O(n) draw calls
- **Memory** — Only holds 2–3 intermediate canvases simultaneously; no base64 strings retained after processing
- **Caching** — `Cache-Control: no-cache` meta tags ensure fresh loads on GitHub Pages
- **Progressive Enhancement** — Core functionality works without CSS animations or Google Fonts (system font fallback)

---

## Browser Support

| Browser | Minimum Version | Notes |
|---------|----------------|-------|
| Chrome | 60+ | Full support |
| Firefox | 55+ | Full support |
| Safari | 11+ | Full support |
| Edge | 79+ | Full support |
| Mobile Chrome/Safari | Current - 2 | Touch drag-and-drop supported |

Requires: `HTMLCanvasElement.toDataURL()`, `CanvasRenderingContext2D.drawImage()`, `FileReader`, Drag-and-Drop API.

---

## Deployment

### GitHub Pages (Current)

The `master` branch is deployed to GitHub Pages at:
**https://ayushshukla73.github.io/digital-geometric-glitch-art/**

**Deployment Steps**
1. Push to `master` branch
2. GitHub Pages automatically builds from root (`/` in `master`)
3. No build step — `index.html` served directly

**Custom Domain** (optional)
1. Add `CNAME` file with domain
2. Configure DNS `CNAME` → `ayushshukla73.github.io`
3. Enable "Enforce HTTPS" in repo settings → Pages

---

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Quick Start for Contributors
1. Fork the repository
2. Create a feature branch: `git checkout -b feat/amazing-feature`
3. Make changes to `index.html` (single-file app)
4. Test locally with any static server
5. Ensure accessibility checks pass (keyboard nav, reduced motion, contrast)
6. Submit a Pull Request

### Development Guidelines
- **Single file** — All code lives in `index.html`; keep it organized with clear sections
- **Vanilla only** — No frameworks, build tools, or external JS dependencies
- **Progressive enhancement** — Core features work without animations/fonts
- **Accessibility first** — Every new UI element must be keyboard accessible
- **Performance** — Avoid memory leaks; clean up object URLs, limit canvas count

---

## Code of Conduct

This project follows the [Contributor Covenant 2.1](CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

---

## Security

See [SECURITY.md](SECURITY.md) for vulnerability reporting and security policy.

**Key Points**
- Client-side only — no server, no data leaves the browser
- No user accounts, no authentication, no tracking
- Images processed entirely in memory via Canvas API
- Content Security Policy compatible (inline styles/scripts only)

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

### Recent Highlights
- **v1.2.0** — Cache-control headers, contributor cleanup, documentation overhaul
- **v1.1.0** — Editorial redesign (Syne + JetBrains Mono), tabbed results, asymmetric upload card, scanline/shimmer effects
- **v1.0.0** — Initial pipeline implementation, GitHub Pages deployment

---

## License

Distributed under the MIT License. See [LICENSE](LICENSE) for details.

```
MIT License

Copyright (c) 2024 Ayush Shukla

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Acknowledgments

- **Syne** — Display font by Bonjour Monde (Google Fonts)
- **JetBrains Mono** — Mono font by JetBrains (Google Fonts)
- **Canvas API** — MDN Web Docs for reference implementations
- **Glitch Art Community** — Inspiration for geometric recombination techniques

---

<p align="center">
  Made with vanilla JS + Canvas API • Deployed on GitHub Pages
</p>