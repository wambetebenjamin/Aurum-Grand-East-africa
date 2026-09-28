# AURUM GRAND

> *Where every moment is golden.*

A single-page, multi-view website for AURUM GRAND — a five-star hotel brand.
Pure HTML / CSS / ES modules. No framework, no bundler, no build step.

## Files

| Path | What it is |
| --- | --- |
| `index.html` | The entire site — markup, design system, router, and the WebGL engine |
| `assets/img/` | Photography (hero suite, suites, amenities, dining, spa, events) |

## Run it

```bash
python3 -m http.server 8000
# or: npx serve .
```

Then open `http://localhost:8000`. Hash routing means any deep link works:
`#/`, `#/rooms`, `#/dining`, `#/spa`, `#/events`, `#/contact`.

## What's inside

**Six WebGL scenes (Three.js, one per route, rebuilt on navigation)**
- **Home** — 96 glossy gold & pearl spheres sweep in from offscreen into a floating
  chandelier, drop and bounce on the marble floor (`floorY = -4`, restitution `0.75`),
  reassemble into a 5-star crown, then spiral upward like rising champagne.
- **Rooms** — 46 gold key fobs orbit a glowing orb; the cursor pushes them away and
  scrolling settles them into a floor-plan grid.
- **Dining** — `LatheGeometry` wine glasses filled with red and champagne spheres
  tumble, collide, and audibly clink; scrolling sets them around a dining table.
- **Spa** — iridescent matte toroid spheres drift like steam and are drawn gently
  toward the cursor (thin-film interference simulated by cycling film thickness).
- **Events** — a 120-sphere ballroom chandelier that lifts and expands with scroll.
- **Contact** — a rotating gold beacon over a field of drifting light.

**Physics / feel constants** — `LERP 0.07`, `WHEEL_MULT 0.85`,
repulsion radius `2.2`, repulsion strength `0.06`.

**Ambience controls** — four palettes (Obsidian Gold, Champagne Rose,
Midnight Sapphire, Emerald Marble) that repaint the background *and* the 3D
materials, plus motion / sound / bloom switches. The background also morphs
continuously as you scroll through the three specified gradients.

**Concierge (WhatsApp)** — every call to action hands off to the hotel's
WhatsApp line: the floating concierge orb, "Reserve", the availability widget,
each suite's "Book", table and treatment bookings, the events enquiry and the
contact form. Each opens a pre-written, context-aware message (suite, dates,
guests, treatment…). The number itself is never printed anywhere in the UI —
it lives only in the `WA_NUMBER` constant at the top of the module script.

**Also** — loader with the four-step sequence, 32s testimonial marquee, gold
footer ticker, snap-scrolling amenity strip, 1.8s blur-and-rise reveals,
glassmorphic availability widget, English / French / Kiswahili / Arabic
(including `dir="rtl"`), and graceful degradation when WebGL or the CDN is
unavailable.
