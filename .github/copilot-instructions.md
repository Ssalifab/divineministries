# Copilot Instructions - Divine Power Of Christ Ministries Website

## Project Overview
A responsive single-page church website built with **HTML5**, **Bootstrap 5.3**, **CSS3**, and vanilla JavaScript. The site showcases ministry information, leadership messages, gallery/video media, branch locations, services, and donation/prayer request functionality.

**Key Files:**
- [index.html](../index.html) - Main website (1489 lines, embedded styles + scripts)
- [css/styles.css](../css/styles.css) - External CSS fallback (266 lines, duplicates index.html styles)
- [scripts/index.yaml](../scripts/index.yaml) - Unrelated SACCO website file (ignore for this project)

## Architecture Patterns

### Single-Page Layout with Section Navigation
The site uses anchor links (`#home`, `#about`, `#leadership`, etc.) to navigate between major sections. All sections are in one HTML file with sticky navbar that dynamically highlights active links based on scroll position.

**Sections (in order):**
- Hero (welcome banner with CTA buttons)
- About (church background + carousel)
- Leadership (3 collapsible leader cards with messages)
- Media (gallery grid 12-column layout + video player)
- Services (3 service cards with times)
- Branches (6 location cards across Uganda)
- Ministries (4 ministry options with icons)
- Give/Donations (3 giving types: tithes, offerings, gifts)
- Contact (form + info cards)
- Footer (links, schedules, social)

### Design System (CSS Variables)
Located in `:root` block in `<style>` tag:
```css
--primary: #2c5aa0 (blue)
--secondary: #d4af37 (gold accent)
--light: #f8f9fa
--dark: #0b1220
--muted: #64748b
--accent: #8b0000 (dark red)
--radius: 16px
--shadow: 0 12px 30px rgba(2, 6, 23, .10)
```

Use these CSS variables instead of hardcoding colors. Gold accents (`--secondary`) appear in underlines, badges, and footer headings. Primary blue is used for buttons, icons, and active states.

### Component Class Naming Conventions
- **Cards:** `.service-card`, `.event-card`, `.leader-card`, `.soft-card` — all have hover transform (`translateY(-6px)`)
- **Sections:** `.py-5` (padding-y) + alternating `.bg-light` for rhythm
- **Gallery Items:** `.g-item` with responsive grid spans (`.g1` through `.g6`) — use `data-bs-toggle="modal"` for lightbox
- **Forms:** `.contact-form`, `.prayer-request-form` — light background with shadow
- **Buttons:** `.btn-primary` (filled), `.btn-outline-primary`, `.btn-outline-light` — always `border-radius: 999px` (pill shape)

## Critical Developer Workflows

### Adding New Gallery Images
1. Place image in `/img/` folder
2. Add `<div class="g-item g[1-6]">` in gallery grid with:
   - `data-bs-toggle="modal" data-bs-target="#imageModal"`
   - `data-img="/img/filename.jpg" data-title="Label"`
   - `<img src="/img/filename.jpg" alt="description">`
   - Badge overlay with category icon

**Note:** Grid spans are responsive (`g1=col-span-6 desktop`, `g2-g3=col-span-3`, `g4-g6=col-span-4`). Mobile defaults to full width.

### Managing Modal Dialogs
Three modals exist with Bootstrap 5 data-attributes:
- `#imageModal` — gallery lightbox (dynamically populated via `data-img`/`data-title`)
- `#donationModal` — showing bank + Airtel Money payment details + copy button function
- `#prayerModal` — prayer request form submission

Modal content is populated via JavaScript event listeners (`show.bs.modal`, `hidden.bs.modal`).

### Form Submission Patterns
Both `#contactForm` and `#prayerForm` use inline JavaScript listeners:
- Prevent default, show alert, reset form
- Prayer form also closes modal after submission
- **No backend integration** — alerts confirm submission only (data not persisted)

### Smooth Scrolling & Active Navigation
JavaScript automatically:
1. Sets `.active` class on nav link matching current scroll section
2. Closes mobile menu collapse when clicking anchor links
3. Scrolls to section top - 70px (navbar height offset)

**Do not change:** Scroll event listeners or section `id` attributes — navigation depends on them.

## Project-Specific Conventions

### Leadership Messages (Collapsible Pattern)
Each leader card has a hidden `.collapse` div toggled via button with `data-bs-target="#msgLeader"` (unique IDs per leader). Messages use nested `<p>`, `<li>`, `<ol>` for scripture references and structured content.

### Branch Locations (Static Data)
6 locations hardcoded as event cards with `.event-date` header (location name) and `.card-body` (address). No database — treat as content update when adding branches.

### Donation Payment Methods
Two payment types (not integrated, display-only):
1. **ABSA Bank:** Account `6008605694`
2. **Airtel Money:** Merchant ID `6993587` — QR code image at `/img/Merchant-ID.jpg`

`copyBankDetails()` function copies both details to clipboard via modern Clipboard API with fallback to deprecated `document.execCommand('copy')`.

### Video Player
Single embedded `<video>` element with `controls preload="metadata" poster="/img/video-poster.jpg"` in `.video-frame` (aspect-ratio 16/9). Supports one MP4 at `/videos/sermon.mp4`.

### Responsive Breakpoints
- **Desktop (≥992px):** Full layout, gallery grid 12-column
- **Tablet (<992px):** Reduced image heights, gallery spans adjust
- **Mobile (<576px):** Full-width gallery items, stacked layout

All responsive rules in media queries at end of `<style>` block.

## Integration Points & External Dependencies

### External Libraries (CDN)
- **Bootstrap 5.3.0:** CSS + JS bundle (navbar collapse, modals, carousel, grid)
- **Font Awesome 6.5.2:** Icons (`.fa-solid`, `.fa-church`, `.fa-phone`, etc.)
- **Google Fonts:** Segoe UI, system fonts

### Images & Media
- Must be in `/img/` folder with exact paths: `/img/gallery-1.jpg`, `/img/Apostle-Ambrose.jpg`, etc.
- Carousel images: `churchInterior.jpg`, `churchExterior.jpg`, `membersWorshipping.jpg`
- Video: `/videos/sermon.mp4`
- Poster: `/img/video-poster.jpg`

**Missing images will break layout.** Use placeholder image services if developing without assets.

### No Build Process
This is a static site — no npm, no bundling, no compilation. Serve `index.html` directly via HTTP server. CSS and JS are embedded or linked via CDN.

## Common Editing Tasks & Examples

### Update Ministry Hours (Services Section)
```html
<!-- Find: <div class="col-md-4"> with Sunday Worship -->
<p class="text-muted mb-0"><i class="fas fa-clock me-2"></i>8:00 AM to 1:00 PM</p>
<!-- Change time as needed -->
```

### Add New Branch
Copy an event card block and update:
```html
<div class="event-date">
    <h5 class="mb-0">NEW_BRANCH_NAME</h5>
    <div class="small">Divine Power Of Christ Church</div>
</div>
<div class="card-body">
    <h5 class="card-title fw-bold">Location</h5>
    <p class="card-text text-muted mb-0">NEW_ADDRESS</p>
</div>
```

### Modify Button Styling
Change `.btn-primary`, `.btn-outline-primary` classes or update CSS variables in `:root`. All buttons use `border-radius: 999px` — do not override to maintain consistency.

### Update Leadership Messages
Find the `.collapse` div for each leader (IDs: `#msgLeader`, `#msgMusic`, `#msgAdmin`). Edit inner `.small` content. Keep structure (`<p>`, `<hr>`, `<em>` attribution) consistent.

## Testing & Deployment Notes

- **No test framework** — manually verify responsive layout, modal functionality, form alerts
- **Mobile-first inspection:** Test at 576px, 768px, 992px breakpoints
- **Accessibility:** Verify color contrast (primary blue #2c5aa0 meets WCAG AA on light backgrounds)
- **Browser support:** Bootstrap 5 supports IE11 and modern browsers; Font Awesome icons require modern CSS support

**Deployment:** Copy entire folder to web server. Ensure `/img/` and `/videos/` directories exist and are populated before going live.
