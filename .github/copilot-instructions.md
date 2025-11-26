# bettterFunds Landing Page - AI Agent Instructions

## Project Overview
Single-file HTML landing page for bettterFunds (fintech investment platform). The site is multilingual, fully responsive, and features dynamic sections with animations and i18n support.

**Key URLs**: No backend required for this landing page; it's a static HTML file with client-side i18n.

---

## Architecture & Design Patterns

### Language & Stack
- **Single File**: `index.html` (no build process, no dependencies)
- **Styling**: Tailwind CSS (CDN + inline config)
- **Fonts**: Google Fonts (Montserrat, Roboto, Cairo for Arabic)
- **Languages**: French (default), English, German, Arabic

### Tailwind Configuration
Inline custom config in `<script>` tag defines:
- **Brand Colors**: `brand.blue (#0000FF)`, `brand.dark (#050511)`, `brand.surface (#F8FAFC)`, `brand.text (#1F2937)`
- **Font Families**: `sans (Roboto)`, `display (Montserrat)`, `arabic (Cairo)`
- **Custom Animations**: `fade-in-up`, `float` (uses keyframes defined inline)

Use these color/animation names in classes. Do NOT hard-code hex values.

### CSS Patterns
**Inline styles (`<style>` block):**
- `.slider-slide` + `.active`: controls visibility/opacity for hero carousel (5-second auto-rotation)
- `.reveal` + `.active`: scroll-triggered fade-in animations
- `.blue-underline::after`: decorative underline utility
- `.bg-grid`: hero section grid background with radial mask
- **RTL overrides**: `html[dir="rtl"]` flips borders, padding, margins, text alignment for Arabic (applied via `changeLanguage('ar')`)

### Component Structure
**Seven Main Sections:**
1. **Navigation**: Fixed top bar with logo, desktop/mobile menus, language selector
2. **Hero**: Auto-rotating slides (3 variations), CTA buttons, grid background
3. **Why**: Problem context with bar charts visualization
4. **What**: 3-card solution layout (Identification, Matching, Readiness)
5. **When**: Vertical timeline (roadmap with active state on step 2)
6. **Impact**: Stats boxes + feature list (dark background)
7. **Team**: Team member placeholders + partner logos
8. **Join**: Email signup form + contact info
9. **Footer**: Links, copyright

---

## Internationalization (i18n) System

### Translation Object Structure
Located in final `<script>` block: `translations` object with keys:
- `fr`, `en`, `de`, `ar` (fully populated)
- Each key contains ~80 translation strings

### Implementation
- All user-facing text wrapped in `<span data-i18n="KEY">Default Text</span>`
- `changeLanguage(lang)` function:
  1. Loops through all `[data-i18n]` elements
  2. Updates `innerText` from `translations[lang][key]`
  3. Sets `html[dir="rtl"]` for Arabic only
  4. Updates UI language buttons

**To add new text:**
1. Add `data-i18n="unique_key"` to the element
2. Add entry to all 4 language objects: `key: "text"`
3. Do NOT modify inline; use data attribute for consistency

---

## JavaScript Patterns

### Mobile Menu Toggle
```js
btn.addEventListener('click', () => {
    menu.classList.toggle('hidden');
});
```
Only active on `md:` breakpoint (hidden on desktop).

### Hero Slider
- **Mechanism**: `setSlide(n)` manually sets index, `showSlides()` auto-rotates every 5 seconds
- **State**: `slideIndex` global, `.slider-slide.active` + `.slider-dot` background color indicate current slide
- **Behavior**: Loops infinitely; clicking dots resets auto-rotation timer

### Scroll Reveal
Uses `IntersectionObserver` to detect when sections enter viewport:
- Adds `.active` class to `.reveal` elements
- Triggers CSS transition to visible state
- Threshold set to `0.1` (10% visibility)

### Language Cycling (Mobile)
`cycleLanguage()` rotates through `['fr', 'en', 'de', 'ar']` on button click.

---

## Responsive Design

### Breakpoints (Tailwind defaults)
- **Mobile-first**: Base styles for small screens
- **`md:` (768px+)**: Desktop navigation, grid layouts
- **`lg:` (1024px+)**: Larger hero text, max-width containers

### Common Utilities
- `max-w-7xl`: outer container (1280px)
- `max-w-6xl`, `max-w-5xl`: section-specific widths
- `grid md:grid-cols-2`, `md:grid-cols-3`: responsive grids
- `flex flex-col sm:flex-row`: stack→row on tablet+

---

## Key Files & Patterns

| File | Purpose | Pattern |
|------|---------|---------|
| `index.html` | All content + logic | Single file; CSS + JS inline |
| Tailwind config (in `<script>`) | Theme colors, fonts, animations | Use `brand-*` classes, not hex |
| `.slider-slide`, `.reveal` | Animation triggers | Add `.active` class via JS |
| `translations` object | i18n strings | Update all 4 language keys together |
| Mobile menu (`#mobile-menu`) | Responsive navigation | Toggle `.hidden` class |

---

## Critical Developer Notes

### Do NOT
- Hard-code colors; use Tailwind tokens (`text-brand-blue`, `bg-brand-surface`)
- Modify the translation object structure; add keys to all 4 languages
- Remove the `data-i18n` attributes; they're the i18n mapping
- Change HTML structure without updating CSS for RTL support

### When Adding Features
1. **New text?** → Add to `translations` all 4 languages + `data-i18n` attribute
2. **New section?** → Follow 7-section template; add `.reveal` class for scroll animation
3. **New animation?** → Define in Tailwind `keyframes` config, not inline styles
4. **RTL support?** → Add CSS rules under `html[dir="rtl"]`
5. **New language?** → Duplicate entire language object in `translations`

### Common Edits
- **Update CTA text**: Find `data-i18n="cta_main"`, update all 4 language keys
- **Change brand color**: Update `brand.blue` in Tailwind config (one place)
- **Adjust slide timing**: Change `5000` in `setSlide()` function
- **Add team member**: Duplicate team member block, update text + `data-i18n` keys

---

## i18n Configuration Details

**Default Language**: French (`changeLanguage('fr')` on load not explicitly called, but French translations are defaults)

**Language Selector**:
- Desktop: Dropdown menu (`:hover` trigger)
- Mobile: Cycle button (rotates through languages)

**Direction Handling**:
- Arabic sets `html[dir="rtl"]` globally
- CSS rules under `html[dir="rtl"]` override LTR layouts (borders, padding, text-align)

---

## Testing Checklist

- [ ] All 4 languages render correctly (check dropdown)
- [ ] RTL mode (Arabic) flips borders/padding as expected
- [ ] Mobile menu toggles on small screens
- [ ] Hero slides auto-rotate and manual dots work
- [ ] Scroll reveal animations trigger at correct viewport position
- [ ] No hard-coded colors—all from Tailwind theme
- [ ] All i18n keys present in all 4 language objects

---

## External References
- Tailwind CDN: `https://cdn.tailwindcss.com`
- Google Fonts: Montserrat, Roboto, Cairo
- **No backend/API calls** in this landing page
