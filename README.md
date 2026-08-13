# Red Team Operator — Resume / Portfolio

A single-file, dark terminal/cyberpunk résumé site for a **Red Team Operator / Offensive Security Researcher**.
Everything (HTML, CSS, JS) lives in `index.html` — no build step, no dependencies, no framework.

```
redteam-resume/
├── index.html   # the entire site
└── README.md    # this file
```

---

## Viewing it

### Option 1 — just open it

```bash
open ~/redteam-resume/index.html          # macOS
xdg-open ~/redteam-resume/index.html      # Linux
start %USERPROFILE%\redteam-resume\index.html   # Windows
```

Double-clicking the file in Finder/Explorer works too.

### Option 2 — local web server (recommended)

A server avoids `file://` quirks and matches how it will behave when deployed.

```bash
cd ~/redteam-resume
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

Other one-liners, if you prefer:

```bash
npx serve .          # Node
php -S localhost:8000
ruby -run -e httpd . -p 8000
```

---

## Deploying

Since it's one static file, anything that serves static content works:

| Host | How |
|---|---|
| **GitHub Pages** | Push the folder to a repo → Settings → Pages → deploy from branch root |
| **Netlify** | Drag the folder onto <https://app.netlify.com/drop> |
| **Vercel** | `npx vercel` inside the folder |
| **Cloudflare Pages** | Connect the repo, leave the build command blank |
| **Any VPS** | Drop `index.html` into your nginx/Apache web root |

---

## What's in it

**Sections** — Hero, About (`whoami`), Skills, Experience, Certifications, Contact.

**Aesthetic**
- Black background with neon green `#00ff41` and electric blue `#00d4ff` accents
- JetBrains Mono via Google Fonts (falls back to Fira Code → system monospace)
- CRT scanline overlay, moving scan sweep, and vignette
- Subtle Matrix rain on a background canvas (throttled to ~14fps, pauses when the tab is hidden)
- Faint grid backdrop with a radial mask

**Animations**
- Typing effect on the hero tagline, cycling through four lines with a blinking block cursor
- Occasional RGB glitch pass on the name
- Glowing borders that intensify on hover; skill cards have a cursor-following glow
- Scroll-triggered reveals with staggered children (IntersectionObserver)
- Animated count-up on the hero stats
- Pulsing status indicators

**Behaviour**
- Fully responsive; collapses to a hamburger menu under 820px
- Scrollspy highlights the active nav section
- Keyboard accessible with a skip link and visible focus rings
- Honours `prefers-reduced-motion` — disables the rain, sweep, typing, and reveals
- Print stylesheet: prints as a clean light-on-white résumé (Cmd/Ctrl+P → Save as PDF)

---

## Customising

Open `index.html` and edit in place. Landmarks are marked with comment banners like
`<!-- ==================== SKILLS ==================== -->`.

**Colours** — the `:root` block at the top of the `<style>` tag:

```css
--green: #00ff41;   /* primary accent */
--blue:  #00d4ff;   /* secondary accent */
--bg:    #050505;   /* page background */
--text:  #c4d3c4;   /* body copy */
```

**Name & tagline** — search for `<h1 class="name">`. Keep the `data-text` attribute in sync
with the visible text or the glitch layers will render the wrong string.

**Typed lines** — the `TAGLINES` array near the top of the `<script>`:

```js
var TAGLINES = [
  'Breaking into things — with a signed scope and a written report.',
  ...
];
```

**Hero stats** — `<b data-count="7">`; change `data-count`, and `data-suffix` for a trailing `+`.

**Skills** — four `<article class="skill-card">` blocks. Add `class="skill-card blue"` to switch a
card to the blue accent. Each skill is one `<li>` inside `<ul class="tags">`.

**Experience** — `<article class="job">` entries inside `.timeline`. Add `class="job current"` to
give an entry the pulsing dot.

**Certifications** — `<div class="cert">` blocks. Swap `<span class="badge ok">Verified</span>` for
`<span class="badge wip">In Progress</span>` as needed.

**Contact** — the `<ul class="links">` list. The GitHub / LinkedIn / X entries are placeholders
(`yourhandle`) — replace both the `href` and the visible text. The PGP fingerprint is a zeroed
placeholder; drop in your real one or delete the `.pgp` block.

**Résumé PDF** — the Résumé link currently triggers the browser print dialog. To link a real file,
put `resume.pdf` next to `index.html` and change `href="#"` to `href="resume.pdf"`.

> The experience, certification, and stat content is realistic sample material shaped around a red
> team career path. Replace it with your own before publishing anything.

---

## Notes

- **Fonts need a network connection.** The Google Fonts `<link>` in `<head>` pulls JetBrains Mono at
  load time. Offline, it falls back to Fira Code and then the system monospace — the layout holds up
  fine either way. To go fully self-contained, download the woff2 files and swap the `<link>` for a
  local `@font-face` rule.
- Everything else is entirely self-contained: no CDN scripts, no trackers, no external images. The
  favicon is an inline SVG data URI.
- Tested in current Chrome, Firefox, and Safari. Uses `IntersectionObserver` and `backdrop-filter`,
  both of which degrade gracefully — content still renders without them.
