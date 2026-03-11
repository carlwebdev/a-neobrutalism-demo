# CLAUDE.md — AI Assistant Guide for a-neobrutalism-demo

## Project Overview

This is a **pure static HTML/CSS/JavaScript website** demonstrating neobrutalism design aesthetics. It serves as a portfolio/marketing site for a fictional design studio called "Brutal Design" by Carl Johansson. There is no build system, no package manager, and no framework — files are served directly.

---

## Repository Structure

```
/
├── index.html          # Landing page (snap-scroll, ~986 lines)
├── about.html          # About/company page
├── contact.html        # Contact form + studio locations
├── locations.html      # Global studio locations with maps
├── services.html       # Services overview + case studies
├── staff.html          # Staff directory
├── styleguide.html     # Design system reference (components, colors, typography)
├── assets/
│   ├── css/
│   │   └── style.css   # Single stylesheet for entire site (~3,657 lines)
│   └── img/
│       ├── hero-bg.svg
│       ├── logoipsum-332.svg
│       └── avatar.jpeg
├── README.txt
└── TASKS.xlsx
```

**No** `package.json`, `node_modules`, `tsconfig.json`, `.eslintrc`, or build config of any kind.

---

## Tech Stack

| Layer      | Technology                                           |
|------------|------------------------------------------------------|
| Markup     | HTML5 (semantic elements)                            |
| Styling    | CSS3 (custom properties, Grid, Flexbox, `clamp()`)   |
| Scripting  | Vanilla JavaScript ES6+ (inline `<script>` tags)    |
| Fonts      | Google Fonts — Work Sans (variable, 100–900)         |
| Icons      | Material Symbols Outlined + WordPress Dashicons (CDN)|
| Maps       | Google Maps embed (contact page)                     |

---

## Development Workflow

### Running Locally

Serve from the project root with any static file server:

```bash
# Python (built-in)
python3 -m http.server 8080

# Node.js (if installed)
npx serve .

# VS Code: use the "Live Server" extension
```

Then open `http://localhost:8080` in a browser.

### No Build Step

There is nothing to compile, bundle, or transpile. Edit HTML/CSS/JS files and refresh the browser.

### No Tests

There are no automated tests. Manual browser testing is the only testing method.

---

## CSS Architecture

All styles live in `assets/css/style.css`. The file is structured as:

1. CSS custom properties (design tokens) at `:root`
2. Reset / base styles
3. Shared layout components (`.wrapper`, `.page-header`, `.page-nav`, `.brutal-footer`)
4. Component styles (buttons, forms, cards)
5. Per-page section styles
6. Dark mode overrides (`body.dark-mode`)
7. Responsive media queries

### CSS Custom Properties (Design Tokens)

```css
:root {
  --c-bg:          #9c7bff;   /* Primary background — purple */
  --c-bg-alt:      #00ff9f;   /* Secondary background — cyan */
  --c-accent:      #f8ff1d;   /* Accent highlight — yellow */
  --c-link:        #000000;
  --c-text:        #000;

  --border-style:  solid;
  --border-color:  black;
  --border-width:  5px;
  --border-radius: 8px;
  --box-shadow:    5px 5px black;   /* Neobrutalism offset shadow */

  --ff: "Work Sans", serif;
}

body.dark-mode {
  --c-bg:         #0f172a;
  --c-bg-alt:     #1f2937;
  --c-text:       #f9fafb;
  --border-color: #f9fafb;
  --box-shadow:   5px 5px 0 #f9fafb;
}
```

Always use these custom properties rather than hard-coded colour values.

### Naming Conventions

CSS class names follow a loose BEM-inspired pattern:

- Block: `.testimonial-card`, `.staff-card`, `.brutal-button`
- Element: `.staff-bio`, `.hero__copy`
- Modifier: `--modifier` suffix (sparse usage)

---

## JavaScript Conventions

All JS is **inline** inside `<script>` tags at the bottom of each HTML file. There are no external `.js` source files.

### Patterns in use

- `document.addEventListener('DOMContentLoaded', ...)` for init
- Intersection Observer API for scroll-based effects
- `window.matchMedia()` for responsive behaviour
- `localStorage` for dark-mode preference persistence
- Optional chaining (`?.`) for safe DOM queries
- ARIA attributes updated programmatically (`aria-expanded`, `aria-pressed`, `aria-current`)

### Key script blocks (index.html)

| Script purpose          | Location (approx. line) |
|-------------------------|-------------------------|
| Scroll dot navigation   | 733                     |
| Secondary scroll nav    | 777                     |
| Search toggle           | 805                     |
| Testimonial slider      | 813                     |
| Mobile nav toggle       | 935                     |
| Dark mode / theme       | 945                     |

---

## Component Patterns

### Adding a New Page

1. Copy an existing page (e.g. `about.html`) as a starting point.
2. Update `<title>`, `<meta>` description, and `<link rel="canonical">` (if present).
3. Set `aria-current="page"` on the correct `<nav>` link.
4. Add page-specific CSS at the bottom of `style.css` within a clearly labelled comment block.
5. Link the new page in the navigation of every existing HTML file.

### Buttons

Use `.brutal-button` for all primary CTA buttons. The offset box-shadow is the defining neobrutalism trait — do not remove it.

```html
<a href="#" class="brutal-button">Click Me</a>
```

### Cards

Cards follow a consistent structure: bold border, offset shadow, high-contrast background from the token palette.

```html
<div class="service-card">
  <h3>Card Title</h3>
  <p>Description text.</p>
</div>
```

### Forms

Use `.brutal-form` wrapper and `.brutal-input` on individual inputs.

---

## Design Principles (Neobrutalism)

When adding or modifying UI, follow these rules:

- **Heavy borders** — use `--border-width` (5px) and `--border-color`
- **Offset box-shadows** — use `--box-shadow` (5px 5px black) instead of blurred shadows
- **High contrast** — backgrounds from the token palette (purple, cyan, yellow); body text is black
- **Bold typography** — Work Sans at heavy weights (700–900); uppercase labels are common
- **No rounded softness** — `--border-radius` is 8px (subtle), not pill-shaped
- **Flat / solid colours** — avoid gradients; avoid `opacity` washes

---

## Accessibility Requirements

Maintain the existing accessibility features when editing:

- Preserve semantic HTML (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`)
- Keep `aria-label` on icon-only buttons
- Update `aria-current="page"` on active nav links
- Keep `aria-expanded` on toggle controls (hamburger, search)
- Use `.screen-reader-text` for visually hidden but announced content
- Provide `alt` text on all `<img>` elements

---

## Responsive Design

| Breakpoint     | Usage                                         |
|----------------|-----------------------------------------------|
| `max-width: 768px` | Mobile layout adjustments, enable testimonial carousel |
| `max-width: 720px` | Styleguide-specific column adjustments      |

Use `clamp()` for fluid font and spacing sizes rather than multiple breakpoints where possible.

---

## Git Workflow

The project uses feature branches merged via pull requests.

```bash
# Create a feature branch
git checkout -b your-branch-name

# Commit
git add <files>
git commit -m "Short imperative description of the change"

# Push
git push -u origin your-branch-name
```

Commit messages follow an imperative style (e.g. "Add contact form validation", "Fix nav overflow on mobile").

---

## What NOT to Do

- Do not introduce a build system, bundler, or package manager unless the project explicitly migrates to one.
- Do not add a JS framework (React, Vue, etc.) without a deliberate architectural decision.
- Do not hard-code colours — always use CSS custom properties.
- Do not remove ARIA attributes or accessibility markup.
- Do not add external JS libraries via `<script>` CDN tags without justification.
- Do not break the neobrutalism visual identity (thick borders, offset shadows, high-contrast palette).
