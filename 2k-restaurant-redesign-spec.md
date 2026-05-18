# 2K Restaurant — Website Redesign Specification
> **For use by an AI coding agent (e.g. Impeccable / OpenCode)**  
> This document is the single source of truth for the redesign. Follow every section closely.

---

## 1. Project Overview

**Client:** 2K Restaurant  
**Goal:** Redesign the existing "view-only" restaurant website into a modern, interactive, full-featured web experience that fixes every identified weakness while retaining and improving the brand identity.  
**Output:** A single-file `index.html` (HTML + CSS + vanilla JS, no build step required) that can be opened in any browser or dropped into a basic hosting service.

---

## 2. Identified Weaknesses to Fix

| # | Weakness | Solution to Implement |
|---|----------|-----------------------|
| 4.1 | No online ordering | Add an **Order Online** flow (cart, item selection, checkout form) |
| 4.2 | Food items have limited details | Each menu card must show name, price, ingredients, portion size, and a short description |
| 4.3 | Images are unattractive / inconsistent | Use consistent placeholder image containers with a warm gradient fallback + uniform card sizes |
| 4.4 | No customer interaction features | Add a **Reviews & Ratings** section with a star-rating submission form |
| 4.5 | No table booking system | Add a **Reserve a Table** form with date, time, party size, and name fields |

---

## 3. Aesthetic Direction

### 3.1 Theme
**Warm Luxury / Upscale Casual** — rich, appetizing, and trustworthy.  
Think candlelit tables, dark wood, amber light. NOT sterile. NOT corporate.

### 3.2 Color Palette (use as CSS variables)
```css
:root {
  --bg-primary:     #1a1208;   /* Very dark espresso brown — main background */
  --bg-card:        #261a0d;   /* Slightly lighter for cards */
  --bg-section:     #0f0b06;   /* Darkest — alternating sections */
  --accent-gold:    #c9933a;   /* Primary accent — gold/amber */
  --accent-warm:    #e8b86d;   /* Lighter gold for hover states, highlights */
  --text-primary:   #f5efe6;   /* Off-white warm — body text */
  --text-muted:     #a08060;   /* Muted warm tan — subtitles, labels */
  --text-heading:   #fdf3e3;   /* Brightest — headings */
  --border-subtle:  #3a2a18;   /* Subtle dividers */
  --success:        #4caf79;   /* Confirmation / success states */
  --danger:         #c94a3a;   /* Errors */
}
```

### 3.3 Typography
- **Display / Logo font:** `Playfair Display` (Google Fonts) — elegant serif, strong personality
- **Headings:** `Playfair Display`, italic for section subtitles
- **Body / UI:** `Lato` (Google Fonts) — clean, readable, pairs well
- **Prices / Numbers:** `Lato`, bold, gold color

```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Lato:wght@300;400;700&display=swap" rel="stylesheet">
```

### 3.4 Visual Details
- Subtle **grain/noise texture** overlay on the body (CSS `::before` pseudo-element with SVG noise, low opacity ~0.04)
- **Gold horizontal rule** (`<hr>` styled as a thin 1px line in `--accent-gold`) between major sections
- Card hover: slight upward `translateY(-4px)` + gold `box-shadow` glow
- Buttons: solid gold background, dark text, no border-radius extremes (use `6px`)
- Section headings: left-aligned with a short gold underline bar (not centered, gives editorial feel)
- Background: pure dark, no image — atmosphere comes from color and typography

---

## 4. Page Structure & Sections

Build the page as a single scrolling HTML page with a **sticky navigation bar**.

### 4.0 `<nav>` — Sticky Navigation
- Logo text: **2K Restaurant** (Playfair Display, gold)
- Nav links (smooth scroll anchors): `Menu | Order Online | Reserve a Table | Reviews | Contact`
- On mobile: collapse to a hamburger menu (pure CSS checkbox toggle or minimal JS)
- Background: `--bg-primary` with slight `backdrop-filter: blur(8px)` and bottom border in `--border-subtle`

---

### 4.1 `#hero` — Hero Section
- Full-viewport-height (`100vh`)
- Background: dark gradient overlay (`linear-gradient(to bottom, rgba(15,11,6,0.7), rgba(26,18,8,0.95))`) over a warm food-themed background image **or** a rich CSS radial gradient mesh if no image is used
- Centered content:
  - Tag line above the name (small caps, muted): `"Est. · Kampala ·  Fine Dining"`
  - Restaurant name: **2K Restaurant** (Playfair Display, large, `--text-heading`)
  - Sub-line: `"Taste the Difference"` (italic, muted)
  - Two CTA buttons side by side:
    - `Order Online` → primary (gold background)
    - `Reserve a Table` → secondary (transparent, gold border)
- Scroll-down chevron animation at the bottom (CSS bouncing arrow)

---

### 4.2 `#menu` — Menu / Food Items

**Layout:** CSS Grid, 3 columns on desktop, 2 on tablet, 1 on mobile.

**Each menu card must include:**
```
[ Food Image or Gradient Placeholder ]
[ Category Badge ]        [ Price Badge ]
Food Item Name            (Playfair Display, bold)
Short Description         (1–2 sentences, Lato, muted)
─────────────────────────────
Ingredients: Chicken, garlic, lemon, herbs...
Portion: Single serving (~350g)
─────────────────────────────
[ Add to Order → ]        (gold button)
```

**Image handling:**
- Use a `<div class="food-img">` with a CSS background gradient as placeholder (warm gradient per category — e.g. orange-brown for meats, green-tint for salads)
- Aspect ratio: 16/9, `object-fit: cover` if real `<img>` tags are used
- All cards must be equal height (use CSS Grid `align-items: stretch`)

**Categories to include (create sample data for at least 8 items across these):**
- Starters
- Main Course
- Grills & BBQ
- Sides
- Desserts
- Drinks

**Sample menu items (use these as real data, agent should not invent random names):**

| Name | Category | Price (UGX) | Description | Ingredients | Portion |
|------|----------|-------------|-------------|-------------|---------|
| Chicken Wings | Starters | 18,000 | Crispy fried wings tossed in our signature spice blend | Chicken wings, paprika, garlic, chili, butter | 6 pieces |
| Beef Burger | Main Course | 25,000 | Flame-grilled beef patty with fresh toppings | Beef patty, lettuce, tomato, cheese, brioche bun | 1 burger |
| Grilled Tilapia | Grills & BBQ | 35,000 | Whole tilapia grilled over charcoal with lemon herbs | Tilapia, lemon, herbs, garlic butter | ~500g |
| Chips (French Fries) | Sides | 8,000 | Golden crispy potato fries, lightly salted | Potatoes, salt, vegetable oil | Large portion |
| Chocolate Lava Cake | Desserts | 15,000 | Warm chocolate cake with a molten center | Dark chocolate, eggs, flour, butter, sugar | 1 piece |
| Passion Fruit Juice | Drinks | 7,000 | Freshly blended passion fruit, chilled | Passion fruit, water, sugar | 500ml glass |
| Pork Ribs | Grills & BBQ | 42,000 | Slow-cooked ribs glazed with BBQ sauce | Pork ribs, BBQ sauce, garlic, brown sugar | Half rack |
| Garden Salad | Starters | 12,000 | Fresh mixed greens with house vinaigrette | Lettuce, cucumber, tomato, onion, vinaigrette | Bowl |

**Category filter tabs** above the grid:
- Buttons: `All | Starters | Main Course | Grills & BBQ | Sides | Desserts | Drinks`
- Active tab: gold background; JS filters the grid by `data-category` attribute

---

### 4.3 `#order` — Order Online / Cart System

This is the most important new feature. Implement a **slide-in cart drawer** from the right side.

**How it works:**
1. User clicks `Add to Order` on any menu card
2. Item is added to an in-memory cart array (JS)
3. A **cart icon** in the nav shows the item count badge (updates live)
4. Clicking the cart icon opens a **right-side drawer** overlay:
   - Lists all items with name, quantity controls (`−` / `+`), unit price, line total
   - Remove item button (×)
   - **Subtotal** at the bottom
   - `Proceed to Checkout` button → shows a checkout form below the cart list

**Checkout form (inside the drawer or a modal):**
```
Full Name        [__________________]
Phone Number     [__________________]
Delivery or Pickup?  ( ) Delivery  ( ) Pickup
Delivery Address [__________________]  (show only if Delivery selected)
Special Notes    [__________________]
[ Place Order ]
```

On `Place Order` click:
- Validate that name and phone are filled
- Show a success message: `"✓ Your order has been received! We'll call you shortly to confirm."`
- Clear the cart

**No payment gateway needed** — this is a "call to confirm" model appropriate for the local market.

---

### 4.4 `#reserve` — Reserve a Table

**Full-width section** with a two-column layout (form left, decorative info right).

**Form fields:**
```
Full Name           [__________________]
Phone Number        [__________________]
Email (optional)    [__________________]
Date                [date picker        ]
Time                [select: 10:00 AM … 10:00 PM, 30min intervals]
Party Size          [select: 1–20 people]
Special Requests    [textarea           ]
[ Reserve My Table ]
```

On submit:
- Validate name, phone, date, time, party size
- Show success: `"✓ Reservation received! We'll confirm your table via phone call."`

**Right column (decorative info panel):**
```
🕐 Opening Hours
Monday – Friday: 9:00 AM – 11:00 PM
Saturday – Sunday: 8:00 AM – Midnight

📍 Location
[Address or "Visit us in Kampala"]

📞 Call Us
[Phone number]
```
Style this column with a gold-border card on a slightly lighter dark background.

---

### 4.5 `#reviews` — Reviews & Ratings

**Layout:** Two parts — display of existing reviews, then a submission form.

**Existing reviews display (hardcode 3 sample reviews as default):**

Each review card:
```
★★★★★  (filled gold stars)
"The grilled tilapia was absolutely perfect. Came here for my birthday and the staff were amazing!"
— Sarah K.  ·  2 weeks ago
```

**Review submission form:**
```
Your Name        [__________________]
Your Rating      [★ ★ ★ ★ ★]  (clickable stars, JS)
Your Review      [textarea           ]
[ Submit Review ]
```

On submit:
- Validate all fields and that a star rating is selected
- Append the new review card to the reviews grid with a smooth fade-in animation
- Show `"✓ Thank you for your review!"`

**Star rating widget (JS):**
- 5 clickable star icons (`★`)
- Hovering highlights stars up to that point (gold)
- Clicking locks the selection
- Selected value stored in a hidden input / JS variable

---

### 4.6 `#contact` — Footer / Contact

Simple dark footer:
- Restaurant name + tagline
- Three columns: **Hours**, **Location**, **Contact**
- Social media icons (Facebook, Instagram, WhatsApp) — use Unicode or simple SVG icons, no icon library dependency
- Copyright line: `© 2024 2K Restaurant. All rights reserved.`

---

## 5. Responsive Breakpoints

| Breakpoint | Layout |
|------------|--------|
| `< 480px` (mobile) | 1 column, full-width forms, hamburger nav |
| `480px – 768px` (tablet) | 2-column menu grid, stacked form+info in reserve section |
| `> 768px` (desktop) | 3-column menu grid, side-by-side layouts |

Use CSS media queries only. No external CSS framework.

---

## 6. JavaScript Requirements

All JS must be **vanilla JS** (no jQuery, no React). Write it in a `<script>` tag at the bottom of `index.html`.

| Feature | JS Needed |
|---------|-----------|
| Menu category filter | Filter `.menu-card` elements by `data-category` |
| Cart add/remove/quantity | Array-based cart state, render function that redraws the drawer |
| Cart drawer open/close | Toggle a CSS class on the drawer overlay |
| Cart item count badge | Update nav badge text on every cart mutation |
| Checkout form submit | Validate + show success message |
| Reservation form submit | Validate date/time, show success |
| Review star widget | Hover + click star selection |
| Review form submit | Append new card to DOM |
| Smooth scroll | `document.querySelectorAll('a[href^="#"]')` → `scrollIntoView` |
| Hamburger menu | Toggle class on mobile nav |

---

## 7. Code Quality Rules

1. **No external CSS frameworks** (no Bootstrap, no Tailwind) — write all CSS from scratch using the design system above
2. **No external JS libraries** — pure vanilla JS only
3. Use **CSS custom properties** (`var(--accent-gold)`) for every color — never hardcode hex values in CSS rules
4. All form inputs must have matching `<label>` elements (accessibility)
5. Use **semantic HTML** — `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`, `<form>`
6. Images: use `<img>` with `alt` text, fall back gracefully to gradient `<div>` if no src
7. The entire site must work with **zero network requests** except Google Fonts (the CDN link in `<head>`)

---

## 8. File Output

Produce a **single file**: `index.html`

Structure:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- meta, title, Google Fonts link -->
  <style>
    /* ALL CSS here — organized by section */
  </style>
</head>
<body>
  <!-- nav, hero, menu, order, reserve, reviews, contact/footer -->
  <script>
    // ALL JS here
  </script>
</body>
</html>
```

---

## 9. Agent Instructions Summary

1. Read this entire document before writing a single line of code.
2. Implement **all 5 weakness fixes** — do not skip any section.
3. Use **exactly** the color palette and font choices specified in Section 3.
4. The menu must use the **sample data table** in Section 4.2 — all 8 items, correct prices in UGX.
5. The cart/order system must work end-to-end in the browser with no backend.
6. The reservation and review forms must show success messages on valid submission.
7. Do not use any CSS framework or JS library.
8. Output one clean, well-commented `index.html` file.
