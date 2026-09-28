# AURUM GRAND

A single-page, multi-view website for AURUM GRAND — a five-star hotel brand.
Pure HTML / CSS / ES modules. No framework, no bundler, no build step.

The visual language is harmonized with **Roberto**, the Colorlib hotel &
resort template shipped here as `roberto-master.zip` — its Poppins type scale,
teal/navy palette and component furniture, applied over this site's existing
router and WebGL engine.

## Files

| Path | What it is |
| --- | --- |
| `index.html` | The entire site — markup, design system, router, and the WebGL engine |
| `assets/img/` | Photography — hero suite, seven suites, six amenities, eight menu dishes, spa, events, bar and exterior (27 images, ~4.7 MB) |
| `roberto-master.zip` | The Roberto template, kept as the design source of truth |

## Design system — harmonized with Roberto

Tokens lifted verbatim from `roberto-master/scss/utilities/` and declared at the
top of the `<style>` block in `index.html`:

| Token | Value | Roberto source |
| --- | --- | --- |
| `--primary` | `#1cc3b2` | `$primary` / `$hover` |
| `--heading` | `#2a303b` | `$heading` |
| `--text` | `#636a76` | `$text` |
| `--bg-gray` | `#e8f1f8` | `$bg-gray` |
| `--border` | `#ebebeb` | `$border` |
| `--secondary` | `#afb4bf` | `$secondary` |
| `--dark2` | `#0e2737` | `$dark2` |
| `--overlay` | `rgba(14,39,55,.70)` | `.bg-overlay` |
| `--shadow` | `0 2px 40px 8px rgba(15,15,15,.15)` | `$box-shadow` |
| `--radius` | `2px` | `.roberto-btn` |
| type | **Poppins** 100–900 | `$font-poppins` |

**Typographic scale** follows `_heading.scss` / `_hero.scss`: body 16px/400,
headings 500 at `line-height:1.3`, section `h2` 42 → 36 → 30 → 24px, hero
`h1` 72 → 48 → 42 → 30px, and uppercase eyebrows with `letter-spacing:2px`
in `--primary`.

### Light / dark scoping

The site is a **hybrid**: a dark immersive hero over the live WebGL canvas,
Roberto-light content sections beneath it, and a navy footer. That is driven by
two custom properties rather than duplicated rules —

```css
:root            { --ink-rgb:42,48,59;    --panel-rgb:255,255,255; }  /* light */
.hero, .dining,
footer#foot, …   { --ink-rgb:255,255,255; --panel-rgb:6,20,28;     }  /* dark  */
```

Every `--cream` / `--line` / `--glass` reference in the original stylesheet
resolves through those, so a component keeps correct contrast in either zone.

### Components ported from Roberto

- **Top bar** (`.top-header-area`) — navy, 50px, phone + email left, socials right; slides away on scroll.
- **Header** (`.main-header-area`) — white, 80px, teal full-bleed *Reserve* block that darkens to `#2a303b` on hover.
- **Buttons** (`.roberto-btn`) — 46px, 150px min-width, `2px` radius, teal fill inverting to white on hover; the outlined `btn-2` variant for imagery.
- **Split room rows** (`.single-room-slide`) — 50% photography / 50% navy panel, alternating direction, on `#/rooms`.
- **Booking band** (`.booking-form`) — white card riding the hero's bottom edge, feeding the same WhatsApp handoff.
- **Footer** (`.footer-area`) — navy with teal widget titles and a `#273d4b` rule.

### Accessibility note

Roberto's `#1cc3b2` sits at **2.21:1** against white, both as small teal text
and as a white-on-teal button fill. That is the template's own signature and is
kept verbatim. To trade a little fidelity for WCAG AA, set one token:

```css
:root { --primary-ui:#12897e; }   /* 4.28:1 — repaints fills and teal UI text only */
```

Roberto's `.footer-nav` links (`$text` on `$dark2`, 2.83:1) were moved to
`$secondary` `#afb4bf` — still Roberto's own palette, but 7.41:1.

## Run it

```bash
python3 -m http.server 8000
# or: npx serve .
```

Then open `http://localhost:8000`. Hash routing means any deep link works:
`#/`, `#/rooms`, `#/dining`, `#/spa`, `#/events`, `#/contact`.

## What's inside

**Six WebGL scenes (Three.js, one per route, rebuilt on navigation)**
- **Home** — 96 glossy teal & pearl spheres sweep in from offscreen into a floating
  chandelier, drop and bounce on the marble floor (`floorY = -4`, restitution `0.75`),
  reassemble into a 5-star crown, then spiral upward like rising champagne.
- **Rooms** — 46 teal key fobs orbit a glowing orb; the cursor pushes them away and
  scrolling settles them into a floor-plan grid.
- **Dining** — `LatheGeometry` wine glasses filled with red and champagne spheres
  tumble, collide, and audibly clink; scrolling sets them around a dining table.
- **Spa** — iridescent matte toroid spheres drift like steam and are drawn gently
  toward the cursor (thin-film interference simulated by cycling film thickness).
- **Events** — a 120-sphere ballroom chandelier that lifts and expands with scroll.
- **Contact** — a rotating teal beacon over a field of drifting light.

**Physics / feel constants** — `LERP 0.07`, `WHEEL_MULT 0.85`,
repulsion radius `2.2`, repulsion strength `0.06`.

**Ambience controls** — four palettes (Roberto Teal, Slate Mint,
Midnight Sapphire, Emerald Marble) that repaint the background *and* the 3D
materials, plus motion / sound / bloom switches. The background also morphs
continuously as you scroll through the three specified gradients.

**Concierge (WhatsApp)** — every call to action hands off to the hotel's
WhatsApp line: the floating concierge orb, "Reserve", the availability widget,
each suite's "Book", table and treatment bookings, the events enquiry and the
contact form. Each opens a pre-written, context-aware message (suite, dates,
guests, treatment…). The number itself is never printed anywhere in the UI —
it lives only in the `WA_NUMBER` constant at the top of the module script.

**Also** — loader with the four-step sequence, 32s testimonial marquee, teal
footer ticker, snap-scrolling amenity strip, 1.8s blur-and-rise reveals,
glassmorphic availability widget, English / French / Kiswahili / Arabic
(including `dir="rtl"`), and graceful degradation when WebGL or the CDN is
unavailable.
