# 🕉️ Sivaanandha Jodhida Maiyyam — Astrology Consultation Website

A single-page, richly animated bilingual (Tamil/English) marketing website for a Tamil Nadu–based astrology and jothidam consultation service. Built as a static HTML page styled with Tailwind CSS (CDN), featuring a mystical navy-and-gold aesthetic, a 3D coverflow service carousel, animated mandala backgrounds, live Google Reviews, and a serverless contact form powered by Web3Forms.

---

## 📋 Summary

| | |
|---|---|
| **Type** | Static single-page website  |
| **Purpose** | Lead generation and service showcase for an astrology consultancy |
| **Languages** | Tamil (`ta`) primary content language, English UI labels |
| **Styling** | Tailwind CSS (CDN / Play CDN) + custom CSS |
| **Interactivity** | Vanilla JavaScript (no framework, no bundler) |
| **Fonts** | Cinzel, Lato, Noto Sans Tamil (Google Fonts) |
| **Icons** | Font Awesome 5.15.4 |
| **Hosting** | Any static host (GitHub Pages, Netlify, Vercel, cPanel, etc.) |

---

### Page Sections (in order)
1. **Navbar** — sticky glass navbar with mobile hamburger menu
2. **Hero** — animated mandala, star field, gold-text title, CTA buttons, live counters
3. **Services** — 3D coverflow swiper (7 service cards)
4. **Highlight** — "Birth Chart PDF" featured offer with animated glow ring
5. **Matchmaking** — 10-Porutham marriage compatibility explainer + heart particle animation
6. **About** — astrologer bio and trust badges
7. **Google Reviews** — review cards with read more/less toggle
8. **Contact** — trust badges, contact info, and enquiry form
9. **Footer** — brand, social icons, quick links
10. **Floating Buttons** — persistent Call/WhatsApp buttons

---

## 🧰 Language & Runtime Dependencies

No package manager or build tool is required. All dependencies are loaded via CDN `<script>`/`<link>` tags:

| Dependency | Source | Purpose |
|---|---|---|
| **Tailwind CSS** | `https://cdn.tailwindcss.com` | Utility-first styling (Play CDN, JIT in-browser) |
| **Google Fonts** | `fonts.googleapis.com` | Cinzel, Lato, Noto Sans Tamil |
| **Font Awesome 5.15.4** | `cdnjs.cloudflare.com` | Icon set (phone, WhatsApp, social, UI icons) |


## 🎨 Tailwind Configuration

Tailwind is loaded via the **Play CDN** (`cdn.tailwindcss.com`) and extended inline via `tailwind.config`:

```js
tailwind.config = {
  theme: {
    extend: {
      colors: {
        navy:         '#0D0F1A',
        'navy-light': '#171A2E',
        gold:         '#C8A84B',
        'gold-light': '#E2C97E',
        ivory:        '#F2EDD7',
        mauve:        '#8B6F8E',
        charcoal:     '#2A2D3E',
      },
      fontFamily: {
        cinzel: ['Cinzel', 'serif'],
        lato:   ['Lato', 'sans-serif'],
        tamil:  ['Noto Sans Tamil', 'sans-serif'],
      },
    }
  }
}
```

**Color usage:**
- `navy` / `navy-light` — page background and card backgrounds
- `gold` / `gold-light` — primary accent, buttons, headings, dividers
- `ivory` — body text
- `mauve` — secondary accent (matchmaking section, ambient blobs)
- `charcoal` — subtle section gradients

**Font usage:**
- `font-cinzel` — headings, labels, buttons (serif, ceremonial feel)
- `font-lato` — body copy
- `font-tamil` — all Tamil-language text

> ⚠️ Note: Using the Play CDN means Tailwind compiles in the browser at runtime. For production performance, consider migrating to the Tailwind CLI or PostCSS build so only used utility classes ship in the final CSS.


Website components 

## 🎨 CSS Implementation

All custom CSS lives in a single `<style>` block, organized by feature:

- **Base/reset** — smooth scroll, scrollbar theming, body background/font
- **`.mandala*`** — rotating SVG ring animations at different speeds (`spin-slow` keyframe, 25s/40s/60s, some reversed)
- **`.gold-text`** — gradient text clip for headings
- **`.gold-divider`** — thin fading gradient line used as a section separator
- **`.nav-glass`** — `backdrop-filter: blur()` navbar with translucent background
- **`.service-card` / `.featured-card`** — hover-lift cards with border-glow transitions
- **`.cs-card` / `.cs-swiper-root`** — 3D coverflow carousel card styling (perspective, backface-visibility, transform transitions)
- **`.arrow-1/2/3`** — staggered "nudge" animation for the swipe hint icon
- **`.float-btn` / `.float-phone` / `.float-whatsapp`** — circular floating action buttons with hover scale + icon shake
- **`.stars-bg`** — multi-layer radial-gradient starfield
- **`.form-input`** — dark, gold-accented input styling with focus ring
- **`#mobile-menu`** — `max-height`/`opacity` transition for the collapsible mobile nav
- **`.reveal` / `.reveal.visible`** — scroll-triggered fade-up entrance animation used across nearly every section
- **`.photo-frame`** — glowing bordered circular image frame (About section)
- **`.testimonial-card` / `.google-review-card`** — review card styling with hover elevation and shadow glow
- **`.review-text` / `.review-text.expanded`** — `-webkit-line-clamp` based text truncation for reviews
- **`.heart-particle` / `.ring-burst` / `.spark`** — fixed-position particle elements animated via JS `Element.animate()`
- **`.glow-ring`** — pseudo-element (`::before`/`::after`) conic-gradient border, animated in JS for the Birth Chart PDF card
- **`.begin-matching-btn` / `.explore-service-btn` / `.consult-now-btn` / `.whatsapp-btn` / `.ordervia-whatsapp-btn` / `.chat-whatsapp-btn` / `.submit-btn` / `.google-reviews-btn`** — shared hover/active scale + brightness micro-interaction pattern across all primary CTA buttons
- **`.trust-badges` / `.badge-item` / `.badge-icon`** — pill badge row, responsive gap/padding at `max-width: 639px`
- **`.social-icon`** — circular footer/social icon buttons using an RGB CSS variable (`--c`) for per-icon theming without `color-mix()`

---

## ⚙️ JavaScript Functionality

All logic is vanilla JS, organized into IIFEs and event listeners at the bottom of the page:

### Contact Form Handling
- Intercepts `submit`, disables the button, sends form data as JSON via `fetch()` to ####
- On success: hides the form, reveals the success message
- On failure: shows an inline error message and re-enables the button
- **Send Another Enquiry** button resets the form and toggles views back

### Google Reviews
- `Read More` / `Read Less` toggles full vs. truncated review text using a `data-full` attribute and the `.expanded` class

### 3D Coverflow Services Swiper
- Computes per-card `translateX/Z`, `rotateY`, `scale`, `opacity`, and `blur` based on distance from the active index
- Supports **click-to-select**, **mouse drag**, and **touch swipe** navigation
- Renders navigation dots dynamically and syncs the active dot
- **Autoplay** advances every 6 seconds; resets on any manual interaction
- Dedicated **Swipe** button also advances the carousel

### Heart/Spark Particle Burst
- Triggered on the "Begin Matching" button click (`blastHearts(event)`)
- Spawns an expanding **ring burst**, radiating **sparks**, and floating **heart emoji particles** using the Web Animations API (`Element.animate()`), with randomized angle, speed, gravity, spin, and size for organic motion

### Mobile Menu
- Toggles the `.open` class on `#mobile-menu` and swaps the hamburger/close icon
- Auto-closes when any menu link is clicked

### Navbar & Floating Buttons Scroll Behavior
- Adjusts navbar border opacity after 60px of scroll
- Floating Call/WhatsApp buttons slide out and fade when scrolling down, and reappear when scrolling up (direction-aware via `lastScrollTop` comparison)

### Scroll Reveal
- `IntersectionObserver` adds `.visible` to any `.reveal` element as it enters the viewport (12% threshold)
- Cards inside `.grid` containers receive a staggered `transition-delay` for a cascading entrance effect

### Animated Counters
- Observes elements with `data-target`/`data-suffix` attributes and animates from `0` to the target number using an eased `requestAnimationFrame` loop when scrolled into view

### Glow Ring Animation
- Continuously rotates a conic-gradient angle value and injects it into a dynamically created `<style>` tag to animate the Birth Chart PDF card's glowing border

### Smooth Scroll
- Intercepts all `href="#..."` anchor clicks and scrolls to the target with a fixed offset to account for the sticky navbar

---

## ✅ Browser Compatibility

Built with modern CSS/JS features. Recommended minimum versions:
- Chrome/Edge 90+
- Firefox 88+
- Safari 15+ (for `backdrop-filter`, `-webkit-line-clamp`, CSS custom properties)

> `color-mix()` was intentionally avoided in favor of RGB CSS variables for broader compatibility with older engines and linters.

---

## 🔧 Customization Guide

| To change... | Edit... |
|---|---|
| Brand colors | `tailwind.config.theme.extend.colors` |
| Fonts | `tailwind.config.theme.extend.fontFamily` + Google Fonts `<link>` |
| Phone / WhatsApp numbers | Search & replace `+917200817179` throughout |
| Google Maps location | Update `href="https://maps.app.goo.gl/..."` in Contact section |
| Services offered | Edit `.cs-card` blocks inside `#services` |
| Social media links | Footer `.social-icon` anchors |

---

## 🙏 Credits

- Design & Development: Codex Design/Siddique
