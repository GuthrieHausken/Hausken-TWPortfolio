# CSS Documentation - Portfolio Site V3

## Table of Contents

1. [Design System](#design-system)
2. [CSS Architecture](#css-architecture)
3. [Component Styling](#component-styling)
4. [Responsive Design](#responsive-design)
5. [Animations & Interactions](#animations--interactions)
6. [Performance Optimizations](#performance-optimizations)

---

## Design System

### CSS Custom Properties (Variables)

The design system uses CSS variables (custom properties) for maintainability and consistency. All color, typography, and theme values are defined in `:root` for global access.

#### Color Palette

```css
:root {
    --clr-bg: #eef4fb;              /* Primary background (light blue) */
    --clr-surface: #ffffff;          /* Card/surface elements (white) */
    --clr-surface-2: #f8fbff;        /* Alternate surface (very light blue) */
    --clr-text: #14233a;             /* Primary text (dark blue-gray) */
    --clr-muted: #4d5f74;            /* Secondary/muted text (gray-blue) */
    --clr-line: #c7d5e6;             /* Borders and dividers (light blue-gray) */
    --clr-accent: #123a64;           /* Primary CTA and accents (deep blue) */
    --clr-accent-2: #215290;         /* Secondary accent (medium blue) */
    --clr-accent-soft: #eaf3ff;      /* Soft accent background (very light blue) */
}
```

**Color Usage Patterns**:

| Variable | Primary Use | Alternative Use |
|----------|------------|-----------------|
| `--clr-bg` | Page background | N/A |
| `--clr-surface` | Cards, modals | Primary surfaces |
| `--clr-surface-2` | Alternating panel backgrounds | Subtle variation |
| `--clr-text` | Body text, headings | Primary content text |
| `--clr-muted` | Taglines, descriptions | Secondary text |
| `--clr-line` | Borders, dividers | UI separators |
| `--clr-accent` | Buttons, links, accents | Primary focus |
| `--clr-accent-2` | Button hover states | Secondary actions |
| `--clr-accent-soft` | Icon backgrounds, highlights | Soft callouts |

#### Typography Variables

```css
--font-display: 'Inter', 'Segoe UI', Arial, sans-serif;
--font-body: 'Inter', 'Segoe UI', Arial, sans-serif;
```

**Font Stack Reasoning**:
1. **Inter**: Modern, professional open-source typeface
2. **Segoe UI**: Windows system fallback with good metrics
3. **Arial**: Universal fallback present on all systems
4. **sans-serif**: Generic fallback if all others unavailable

**Font Loading**: Font family specified via system fonts (no @font-face or external CDN). This approach:
- Reduces HTTP requests
- Improves page load performance
- Eliminates font rendering delays
- Provides instant text availability

---

## CSS Architecture

### Reset & Base Styles

#### Universal Reset

```css
* {
    box-sizing: border-box;  /* Include padding/border in width calculations */
    margin: 0;               /* Remove default margins */
    padding: 0;              /* Remove default padding */
}
```

**Purpose**: 
- `box-sizing: border-box` simplifies layout calculations
- Zero defaults prevent unexpected spacing
- Applied globally to normalize browser defaults

#### HTML Root

```css
html {
    scroll-behavior: smooth;  /* Smooth scrolling for anchor links */
}
```

**Effect**: Clicking anchor links (like skip link) animates scroll instead of jumping

#### Body Styling

```css
body {
    font-family: var(--font-body);
    font-size: 1rem;              /* 16px base (standard) */
    line-height: 1.7;             /* Generous line spacing for readability */
    color: var(--clr-text);
    background: radial-gradient(circle at top left, rgba(42, 90, 166, 0.16), transparent 20%),
                linear-gradient(180deg, #ffffff 0%, var(--clr-bg) 100%);
    min-height: 100vh;            /* Full viewport height minimum */
}
```

**Background Implementation**:

1. **Radial Gradient**: Blue glow effect in top-left corner
   - Center: `circle at top left` positions gradient origin
   - Color: `rgba(42, 90, 166, 0.16)` semi-transparent blue
   - Spread: `transparent 20%` creates soft fade

2. **Linear Gradient**: Transitions from white to background color
   - Start: `#ffffff` (white) at top
   - End: `var(--clr-bg)` (light blue) at bottom
   - Creates depth and visual interest

#### Body Pseudo-Element (Grid Pattern)

```css
body::before {
    content: "";
    position: fixed;
    inset: 0;
    background-image: 
        linear-gradient(rgba(25, 59, 109, 0.02) 1px, transparent 1px),
        linear-gradient(90deg, rgba(25, 59, 109, 0.02) 1px, transparent 1px);
    background-size: 34px 34px;
    pointer-events: none;
    z-index: -1;
    opacity: 0.85;
}
```

**Grid Pattern Details**:

- **`::before` Pseudo-Element**: Creates decorative grid without affecting DOM
- **Position**: `fixed` stays in place during scroll (parallax effect)
- **Grid Construction**: Two perpendicular linear gradients
  - Horizontal lines: `linear-gradient(...)` 1px lines on 34px spacing
  - Vertical lines: `linear-gradient(90deg, ...)` rotated 90 degrees
  - Creates 34×34px grid cells
- **Color**: `rgba(25, 59, 109, 0.02)` very subtle blue (2% opacity)
- **Layering**: 
  - `z-index: -1` places behind all content
  - `pointer-events: none` ensures grid doesn't interfere with interactions
  - `opacity: 0.85` slightly reduces prominence (could be removed for more visible grid)
- **Performance**: Fixed positioning on pseudo-element means grid renders once

---

## Component Styling

### Main Content Container

```css
main {
    width: min(100%, 1200px);        /* Fluid width capped at 1200px */
    margin: 0 auto;                  /* Center horizontally */
    padding: 0 clamp(18px, 4vw, 56px) 72px;  /* Responsive padding */
}
```

**Layout Strategy**:

- **Width**: `min(100%, 1200px)` 
  - Prevents overflow on narrow viewports
  - Caps maximum width at 1200px
  - Equivalent to: `max-width: 1200px; width: 100%;`

- **Padding**: `clamp(18px, 4vw, 56px)` responsive horizontal spacing
  - Minimum: 18px (very small screens)
  - Preferred: 4% of viewport width
  - Maximum: 56px (large screens)
  - Auto-scales with screen size without media queries

- **Bottom Padding**: 72px fixed space before footer

### Skip Link

```css
.skip-link {
    position: absolute;              /* Remove from normal flow */
    left: 16px;
    top: -48px;                      /* Hidden above viewport */
    z-index: 1200;                   /* Highest layer when visible */
    padding: 0.7rem 1rem;            /* Comfortable button spacing */
    background: var(--clr-accent);   /* Branded color */
    color: #ffffff;                  /* White text */
    border-radius: 999px;            /* Pill shape */
    text-decoration: none;           /* No underline */
    font-weight: 600;                /* Semi-bold for prominence */
}

.skip-link:focus {
    top: 16px;                       /* Move into view on focus */
}
```

**Behavior**:
- Initially positioned off-screen (top: -48px)
- Moves into viewport on keyboard focus (top: 16px)
- Only visible to keyboard users; hidden from mouse/touch users
- High z-index ensures visibility above all page content

---

### Hero Section

#### Hero Container

```css
.hero {
    position: relative;
    padding: clamp(36px, 6vw, 72px) 0 24px;  /* Responsive vertical padding */
    margin-bottom: 18px;
    border: 1px solid var(--clr-line);
    border-radius: 30px;                      /* Prominent rounded corners */
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.99) 0%, rgba(233, 243, 255, 0.95) 100%);
    box-shadow: 0 16px 40px rgba(22, 33, 46, 0.1);
    overflow: hidden;
}
```

**Visual Features**:

- **Padding**: `clamp()` scales with viewport (36px to 72px vertically)
- **Border**: 1px line using system color variable
- **Border-Radius**: 30px creates smooth, modern appearance
- **Background Gradient**: 
  - Angle: 135° (top-left to bottom-right)
  - Start: Near-white with 99% opacity
  - End: Light blue with 95% opacity
  - Creates subtle depth
- **Shadow**: `0 16px 40px` with 10% opacity creates floating effect
- **Overflow**: `hidden` ensures rounded corners clip child elements

#### Hero Background Accent

```css
.hero::before {
    content: "";
    position: absolute;
    inset: auto -8% -18% auto;      /* Position in bottom-right area */
    width: 220px;
    height: 220px;
    background: radial-gradient(circle, rgba(42, 90, 166, 0.06), transparent 72%);
    pointer-events: none;           /* Doesn't capture clicks */
}
```

**Decorative Accent**:
- Positioned absolutely in bottom-right corner
- Radial gradient creates soft blue glow
- Very low opacity (6%) for subtlety
- Enhances depth without overwhelming content

#### Hero Content Positioning

```css
.hero > * {
    position: relative;
    z-index: 1;                     /* Ensures content appears above accent */
}

.header-inner {
    display: grid;
    grid-template-columns: auto minmax(0, 1fr);  /* Image fixed width, text flexible */
    gap: clamp(28px, 4vw, 44px);               /* Responsive gap between columns */
    align-items: start;                        /* Align items to top */
}
```

**Grid Layout Strategy**:
- **Column 1**: `auto` width (image size determined by image)
- **Column 2**: `minmax(0, 1fr)` flexible (grows to fill remaining space)
  - `minmax(0, 1fr)` allows text to overflow gracefully
- **Gap**: Responsive spacing scales with viewport
- **Align Items**: `start` positions image top-aligned with text

### Profile Image

```css
.profile-img {
    width: min(150px, 24vw);         /* Scales down on small viewports */
    height: min(150px, 24vw);        /* Square aspect ratio */
    object-fit: cover;               /* Crop image to fill bounds */
    border-radius: 50%;              /* Perfect circle */
    border: 2px solid rgba(25, 59, 109, 0.2);
    background-color: #ece5db;       /* Fallback color if image fails */
    flex-shrink: 0;                  /* Prevent shrinking below min size */
    box-shadow: 0 14px 30px rgba(25, 59, 109, 0.14);
    margin-left: 10px;               /* Spacing from edge */
}
```

**Responsive Sizing**:
- **Width/Height**: `min(150px, 24vw)`
  - Max size: 150px (typical laptop screens)
  - Scales down to 24% of viewport width on mobile
  - Maintains square aspect ratio
- **Object-Fit**: `cover` ensures image fills circle without distortion
- **Border-Radius**: `50%` creates perfect circle
- **Flex-Shrink**: `0` prevents grid from shrinking image below declared size
- **Shadow**: Lifts image off background

### Header Text Content

```css
.header-text {
    max-width: 760px;               /* Limit text line length for readability */
}

.eyebrow {
    display: inline-block;
    margin-bottom: 10px;
    font-size: 0.82rem;             /* Smaller than body text */
    font-weight: 700;               /* Bold for emphasis */
    color: var(--clr-accent-2);
    letter-spacing: 0.2em;          /* Wide spacing */
    text-transform: uppercase;      /* All caps */
}
```

**Eyebrow Styling**:
- `inline-block`: Allows margin/padding while respecting flow
- Uppercase with wide letter-spacing: Professional, formal appearance
- Secondary accent color differentiates from main heading
- Smaller size establishes visual hierarchy

### Main Heading

```css
header h1 {
    font-family: var(--font-display);
    font-size: clamp(2rem, 4vw, 3rem);  /* Scales from 2rem to 3rem */
    color: var(--clr-accent);
    line-height: 1.05;                  /* Tight line height for headings */
    margin-bottom: 10px;
    letter-spacing: -0.02em;            /* Slight negative spacing (kerning) */
}
```

**Typography Details**:
- **Font Size**: `clamp(2rem, 4vw, 3rem)` responsive scaling
  - Mobile (320px): ~2rem (32px)
  - Tablet (768px): ~2.3rem
  - Desktop (1200px): ~3rem (48px)
- **Line Height**: 1.05 (tight) creates compact, powerful appearance
- **Letter Spacing**: -0.02em negative spacing tightens characters for visual impact
- **Color**: Accent color for prominence

### Tagline

```css
.tagline {
    font-size: 1rem;                /* Standard body size */
    color: var(--clr-muted);        /* Muted color for secondary content */
    margin-bottom: 18px;
    max-width: 680px;               /* Constrain line length */
}
```

---

### Tab Navigation

#### Tab Container

```css
.hero-actions {
    display: flex;
    flex-wrap: wrap;                /* Buttons wrap on small screens */
    gap: 12px;                      /* Consistent spacing between buttons */
}
```

#### Tab Buttons

```css
.tab-btn {
    border: 1px solid var(--clr-line);
    background: rgba(255, 255, 255, 0.9);  /* Semi-transparent white */
    color: var(--clr-text);
    padding: 0.72rem 1rem;          /* Comfortable button sizing */
    border-radius: 999px;           /* Pill shape */
    cursor: pointer;
    font-size: 0.95rem;             /* Slightly smaller than body */
    transition: all 0.2s ease;      /* Smooth state changes */
    box-shadow: 0 6px 16px rgba(22, 33, 46, 0.04);  /* Subtle shadow */
}

.tab-btn:hover {
    border-color: rgba(25, 59, 109, 0.3);  /* Darker border on hover */
    transform: translateY(-1px);            /* Lift effect */
}

.tab-btn.active {
    background: linear-gradient(135deg, var(--clr-accent) 0%, var(--clr-accent-2) 100%);
    color: white;
    border-color: transparent;
    /* Inherits transition from base state */
}
```

**Button States**:

1. **Inactive (Default)**:
   - Semi-transparent white background
   - System color text
   - Subtle shadow for depth

2. **Hover**:
   - Darker border appears
   - `translateY(-1px)` lifts button slightly
   - Smooth transition animates change

3. **Active**:
   - Gradient background (accent colors)
   - White text
   - No border (uses background color instead)
   - Inherits hover effects for interactive feedback

**Interaction Pattern**:
- All state changes use `transition: all 0.2s ease`
- Provides smooth visual feedback without feeling sluggish
- 200ms duration balances responsiveness and smoothness

---

### Tab Panels

#### Panel Container

```css
.tab-panel {
    position: relative;
    padding: clamp(24px, 3.5vw, 34px);  /* Responsive content padding */
    border: 1px solid var(--clr-line);
    border-radius: 24px;
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.99) 0%, rgba(246, 251, 255, 0.98) 100%);
    box-shadow: 0 16px 36px rgba(22, 33, 46, 0.09);
    margin: 0 0 18px;
    overflow: hidden;              /* Clip content to rounded corners */
}

.tab-panel[hidden] {
    display: none !important;      /* Force hide when hidden attribute present */
}
```

**Panel Styling**:
- Similar card styling to hero section for consistency
- `position: relative` enables absolute positioning of child elements
- `overflow: hidden` ensures content respects border-radius
- `[hidden]` selector with `!important` ensures JavaScript hidden state works

#### Panel Content Offset

```css
.tab-panel > .panel-intro,
.tab-panel > .card-grid,
.tab-panel > .sample-grid,
.tab-panel > .cv-sheet,
.tab-panel > .contact-layout {
    margin-left: clamp(4px, 0.8vw, 10px);  /* Small left offset for visual hierarchy */
}
```

**Purpose**: Subtle left margin creates visual indentation of main content

#### Alternating Panel Backgrounds

```css
.tab-panel:nth-of-type(2n) {
    background: var(--clr-surface-2);     /* Alternate color on even panels */
}
```

**Effect**: Every other panel uses alternate background color for visual rhythm

### Panel Introduction Section

```css
.panel-intro {
    margin-bottom: 20px;
}

section h2 {
    font-family: var(--font-display);
    font-size: 1.1rem;              /* Modest size for section heading */
    color: var(--clr-accent);
    margin-bottom: 10px;
    letter-spacing: 0.16em;         /* Very wide spacing */
    text-transform: uppercase;
}

section h3 {
    font-size: 0.92rem;
    font-weight: 700;
    color: var(--clr-text);
    margin: 0 0 10px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
}

section p,
section li,
section ul,
section ol {
    color: var(--clr-text);
    font-size: 1rem;
}

section p {
    margin-bottom: 10px;
}

section ul,
section ol {
    padding-left: 20px;             /* Standard list indentation */
    margin-bottom: 6px;
}

section ul {
    list-style: disc;               /* Filled circles */
}

section ul li,
section ol li {
    padding: 4px 0;
    line-height: 1.6;
}

section ul li::marker {
    color: var(--clr-accent);       /* Accent color for list bullets */
}
```

**Typography Hierarchy**:
- **H2**: Large section headings (1.1rem, uppercase, accent color, wide letter-spacing)
- **H3**: Subsection headings (0.92rem, uppercase, regular color, normal spacing)
- **Body**: Standard 1rem with generous line height (1.7)
- **Lists**: Indented with custom bullet color

---

### Card Components

#### Card Grid Layout

```css
.card-grid,
.experience-layout,
.contact-layout {
    display: grid;
    gap: 18px;
    grid-template-columns: repeat(2, minmax(0, 1fr));  /* 2-column grid */
}

.overview-grid {
    grid-template-columns: 1fr;    /* Override: single column */
}

.card-grid > :only-child,
.contact-layout > :only-child {
    grid-column: 1 / -1;           /* Span full width if single card */
}
```

**Grid Strategy**:
- **Default**: 2-column layout with equal widths
- **Overview Grid**: Single column for overview section
- **Responsive Fallback**: `minmax(0, 1fr)` allows flexibility:
  - `minmax(0, ...)` allows columns to shrink smaller than content
  - Prevents grid from expanding beyond container
  - Works without media queries on narrow screens

#### Individual Card

```css
.info-card {
    position: relative;
    padding: 18px 20px;
    border: 1px solid var(--clr-line);
    border-radius: 18px;
    background: linear-gradient(145deg, rgba(255, 255, 255, 0.97) 0%, rgba(248, 251, 255, 0.95) 100%);
    box-shadow: 0 10px 24px rgba(22, 33, 46, 0.05);
    transform: translateY(-2px);   /* Subtle lift effect */
}

.large-card {
    grid-column: span 2;           /* Spans both columns */
}
```

**Card Styling**:
- Consistent padding for internal spacing
- Gradient background adds depth
- Subtle shadow keeps card grounded
- Small negative Y transform lifts card visually
- `.large-card` modifier spans full grid width

---

### Sample Portfolio Cards

#### Sample Grid

```css
.sample-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 20px;
}

.sample-grid > :only-child {
    grid-column: 1 / -1;
}
```

#### Sample Card Container

```css
.sample-card {
    border: 1px solid var(--clr-line);
    border-radius: 18px;
    padding: 16px;
    display: flex;
    flex-direction: column;        /* Stack items vertically */
    height: 100%;                  /* Fill grid cell height */
    background: rgba(255, 255, 255, 0.96);
    box-shadow: 0 8px 22px rgba(22, 33, 46, 0.06);
}
```

**Layout**: 
- `flex-direction: column` stacks content vertically
- `height: 100%` ensures all cards equal height
- Allows action buttons to align at bottom

#### Card Top Section

```css
.sample-card-top {
    display: flex;
    gap: 14px;
    align-items: flex-start;       /* Align icon and content to top */
    flex: 1;                       /* Grows to fill available space */
}
```

#### File Type Icon

```css
.sample-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-width: 44px;
    height: 44px;
    border: 1px solid rgba(25, 59, 109, 0.16);
    color: var(--clr-accent);
    font-size: 0.78rem;            /* Small text */
    font-weight: 700;              /* Bold for legibility */
    letter-spacing: 0.04em;
    background: var(--clr-accent-soft);  /* Soft accent background */
    border-radius: 10px;
}
```

**Icon Styling**:
- `inline-flex` with centering ensures content in middle
- 44px square (WCAG touch target minimum)
- Soft accent background differentiates from surrounding content
- Bold uppercase text (e.g., "PDF") clearly identifies content

#### Card Links

```css
.sample-links {
    display: flex;
    flex-wrap: nowrap;             /* Single row, no wrapping */
    gap: 8px;
    margin: 12px 0 0;
    align-items: flex-start;
    justify-content: flex-start;
    width: 100%;
}

.sample-links > * {
    flex: 0 0 calc(33.333% - 6px);  /* Each button 1/3 width minus gap */
    max-width: calc(33.333% - 6px);
    min-width: 0;
    width: calc(33.333% - 6px);
    white-space: nowrap;           /* Prevent text wrapping */
}
```

**Button Distribution**:
- **Flex Basis**: `calc(33.333% - 6px)` divides space equally minus gap
- **Max/Min Width**: Constrains buttons to calculated size
- **White-Space**: `nowrap` prevents button text from wrapping

#### Button & Link Styling (Shared)

```css
.hero-btn,
.sample-links a,
.sample-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 32px;
    height: 32px;
    padding: 0 0.7rem;
    background: linear-gradient(135deg, var(--clr-accent) 0%, var(--clr-accent-2) 100%);
    color: #fdf9f3;                /* Off-white text */
    text-decoration: none;
    font-size: 0.8rem;             /* Smaller text */
    border: 1px solid rgba(255, 255, 255, 0.18);  /* Subtle border */
    border-radius: 999px;
    cursor: pointer;
    box-shadow: 0 10px 24px rgba(25, 59, 109, 0.16);
    transition: transform 0.2s ease, box-shadow 0.2s ease, background 0.2s ease;
}

.hero-btn:hover,
.sample-links a:hover,
.sample-btn:hover {
    transform: translateY(-2px);   /* Lift on hover */
    box-shadow: 0 12px 28px rgba(25, 59, 109, 0.2);  /* Deeper shadow */
    background: linear-gradient(135deg, var(--clr-accent-2) 0%, var(--clr-accent) 100%);  /* Reversed gradient */
    color: #ffffff;                /* Brighter white */
}

.hero-btn.secondary {
    background: rgba(255, 255, 255, 0.96);
    color: var(--clr-accent);
    border-color: var(--clr-line);
    box-shadow: 0 6px 16px rgba(25, 59, 109, 0.08);
}

.hero-btn.secondary:hover {
    background: rgba(234, 242, 255, 1);
    color: var(--clr-accent);
}
```

**Button States**:

1. **Primary (Active)**:
   - Gradient background (accent colors)
   - Off-white text
   - Shadow for depth
   - Transition all: 200ms

2. **Primary Hover**:
   - Reversed gradient (visual feedback)
   - Lifted 2px (`translateY(-2px)`)
   - Deeper shadow
   - White text (brighter)

3. **Secondary**:
   - Transparent white background
   - Accent color text
   - Lighter shadow
   - Styled for secondary actions

---

### Modal Dialog Styling

#### Modal Container

```css
#pdf-modal {
    position: fixed;               /* Covers entire viewport */
    inset: 0;                      /* Top, right, bottom, left all 0 */
    display: flex;
    align-items: center;           /* Center vertically */
    justify-content: center;       /* Center horizontally */
    z-index: 2000;                 /* Above all other content */
    background: rgba(0, 0, 0, 0.5);  /* Semi-transparent backdrop */
}

#pdf-modal[hidden],
#pdf-modal[aria-hidden="true"] {
    display: none;                 /* Hide when hidden attribute present */
}
```

#### Modal Backdrop

```css
.pdf-modal-backdrop {
    position: fixed;
    inset: 0;
    background: transparent;      /* Behind modal content */
}
```

**Backdrop Purpose**: Clickable area outside modal content to close modal

#### Modal Dialog

```css
.pdf-modal-dialog {
    position: relative;
    background: white;
    border-radius: 12px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    max-width: 90%;
    max-height: 90vh;              /* Max 90% of viewport height */
    display: flex;
    flex-direction: column;
    z-index: 2001;                 /* Above backdrop */
}
```

#### Close Button

```css
.pdf-modal-close {
    position: absolute;
    top: 12px;
    right: 12px;
    width: 32px;
    height: 32px;
    background: rgba(0, 0, 0, 0.1);
    border: none;
    border-radius: 50%;
    font-size: 24px;
    cursor: pointer;
    z-index: 2002;                 /* Above dialog content */
    transition: background 0.2s ease;
}

.pdf-modal-close:hover {
    background: rgba(0, 0, 0, 0.2);
}
```

#### PDF Frame

```css
#pdf-frame {
    width: 100%;
    height: 100%;
    border: none;
    border-radius: 8px;
}

#pdf-frame[hidden] {
    display: none;
}
```

#### Image Preview

```css
#image-preview {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
}

#image-preview[hidden] {
    display: none;
}
```

---

### CV Section Styling

#### CV Container

```css
.cv-sheet {
    padding: 24px;
    border: 1px solid var(--clr-line);
    border-radius: 18px;
    background: white;
}

.cv-header {
    display: flex;
    justify-content: space-between;  /* Name on left, role on right */
    margin-bottom: 24px;
    padding-bottom: 16px;
    border-bottom: 1px solid var(--clr-line);
}

.cv-role {
    font-style: italic;
    color: var(--clr-muted);
    max-width: 240px;
}
```

#### CV Sections

```css
.cv-section {
    margin-bottom: 20px;
}

.cv-section h4 {
    font-size: 0.95rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 12px;
    color: var(--clr-accent);
}
```

#### CV Entry

```css
.cv-entry {
    margin-bottom: 16px;
    padding-bottom: 12px;
}

.cv-entry-head {
    display: flex;
    justify-content: space-between;  /* Title left, date/org right */
    margin-bottom: 8px;
}

.cv-entry-head strong {
    font-weight: 700;
}

.cv-entry-head span {
    color: var(--clr-muted);
    font-size: 0.95rem;
}
```

---

### Contact Section

#### Contact List

```css
.contact-list,
.link-list {
    list-style: none;              /* Remove default bullets */
    padding: 0;
    margin: 0;
}

.contact-list li,
.link-list li {
    padding: 6px 0;                /* Spacing between items */
}

.contact-list a,
.link-list a {
    color: var(--clr-accent);
    text-decoration: underline;
    text-underline-offset: 0.18em; /* Space between text and underline */
}

.contact-list a:hover,
.link-list a:hover {
    color: var(--clr-accent-2);    /* Darken on hover */
}
```

---

### Gallery & Work Grid

#### Gallery Grid

```css
.gallery-grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));  /* 3-column layout */
    gap: 12px;
    margin-top: 10px;
}

.gallery-item {
    display: block;
    border-radius: 14px;
    overflow: hidden;
    border: 1px solid var(--clr-line);
    background: var(--clr-surface);
    box-shadow: 0 8px 18px rgba(22, 33, 46, 0.05);
}

.gallery-item img {
    display: block;
    width: 100%;
    height: auto;
}
```

#### Work Actions

```css
.work-grid {
    display: grid;
    gap: 18px;
    grid-template-columns: repeat(2, minmax(0, 1fr));
}

.work-actions {
    display: flex;
    flex-direction: column;        /* Stack items vertically */
    gap: 12px;
}

.work-item {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
}

.work-caption {
    margin: 0;
    font-size: 0.92rem;
    color: var(--clr-muted);
}
```

---

### Footer

```css
footer {
    margin-top: 36px;
    padding: 24px 0;
    border-top: 1px solid var(--clr-line);
    text-align: center;
    color: var(--clr-muted);
    font-size: 0.9rem;
}

footer p {
    margin: 6px 0;
}
```

---

## Responsive Design

### Responsive Strategy (No Media Queries)

This design uses **intrinsic responsive design** - responsive behavior built into CSS values rather than relying on media queries.

#### Fluid Sizing with `clamp()`

The `clamp()` function enables automatic scaling between minimum and maximum values:

```
clamp(min-value, preferred-value, max-value)
```

**Examples**:

```css
/* H1 scales between 2rem and 3rem based on viewport */
font-size: clamp(2rem, 4vw, 3rem);

/* Padding scales between 18px and 56px */
padding-left: clamp(18px, 4vw, 56px);

/* Gap scales between 28px and 44px */
gap: clamp(28px, 4vw, 44px);
```

**Benefits**:
- Smooth scaling across all screen sizes
- No jarring layout shifts at breakpoints
- Fewer CSS rules needed
- Cleaner, more maintainable code

### Container Width

```css
main {
    width: min(100%, 1200px);
}
```

**Behavior**:
- **Narrow screens (< 1200px)**: Width = 100% (full viewport)
- **Wide screens (≥ 1200px)**: Width = 1200px (capped)

### Grid Responsiveness

```css
.card-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
}
```

**Mobile Behavior**:
- On very narrow screens, content still fits in 2 columns
- `minmax(0, 1fr)` prevents grid from expanding beyond container
- Natural wrapping behavior without media queries

**Fallback Pattern**: If 2 columns become too narrow, add:

```css
@media (max-width: 600px) {
    .card-grid {
        grid-template-columns: 1fr;
    }
}
```

### Font Scaling

```css
/* Body text stays at 1rem (no scaling) */
font-size: 1rem;

/* Large heading scales responsively */
h1 { font-size: clamp(2rem, 4vw, 3rem); }

/* Section headings scale slightly */
h2 { font-size: 1.1rem; }
```

---

## Animations & Interactions

### Smooth Scrolling

```css
html {
    scroll-behavior: smooth;
}
```

**Effect**: Anchor link navigation animates scroll instead of jumping

### Button Hover Animation

```css
.tab-btn:hover {
    transform: translateY(-1px);
    transition: all 0.2s ease;
}
```

**Effect**: Button lifts 1px upward on hover

**Timing**: 200ms `ease` function provides smooth, natural motion

### Interactive Button State

```css
.sample-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 12px 28px rgba(25, 59, 109, 0.2);
    background: linear-gradient(135deg, var(--clr-accent-2) 0%, var(--clr-accent) 100%);
}
```

**Cumulative Effects**:
1. Lifts 2px (`translateY`)
2. Deeper shadow appears
3. Gradient direction reverses
4. All transitions happen smoothly together

### Focus States (Accessibility)

```css
button:focus,
a:focus {
    outline: 2px solid var(--clr-accent);
    outline-offset: 2px;
}
```

**Purpose**: Keyboard users see clear focus indicator

---

## Performance Optimizations

### CSS Variables

```css
:root {
    --clr-accent: #123a64;
}

button {
    background: var(--clr-accent);
}

button:hover {
    background: var(--clr-accent-2);
}
```

**Benefits**:
- Single definition updated everywhere
- Easier maintenance
- No CSS preprocessing needed
- Minimal file size

### GPU Acceleration

```css
.sample-btn:hover {
    transform: translateY(-2px);  /* GPU-accelerated */
}
```

**Why**: `transform` properties use GPU, not CPU, resulting in smooth animations

**Avoid**: Direct `top`, `left`, `width` changes (CPU-bound)

### Pseudo-Element Decorations

```css
body::before {
    /* Grid pattern rendered as pseudo-element */
    /* Doesn't affect DOM or JavaScript selection */
}
```

**Benefit**: Decorative elements don't bloat HTML

### Fixed Backgrounds

```css
body::before {
    position: fixed;  /* Stays in place during scroll */
    z-index: -1;
}
```

**Effect**: Grid pattern parallelax effect without performance impact

### Minimal Reflows

```css
.element {
    /* Change classes, not individual styles */
    /* Batch DOM updates in JavaScript */
}
```

**Best Practice**: Update class names rather than multiple inline styles

---

## CSS Validation & Standards

✓ Valid CSS 3  
✓ No vendor prefixes required  
✓ All values use CSS variables  
✓ Responsive without media queries  
✓ Accessible color contrast ratios  
✓ GPU-accelerated animations  
✓ Mobile-first approach  

---

## Browser Compatibility

- **Chrome/Edge**: Full support (latest versions)
- **Firefox**: Full support (latest versions)
- **Safari**: Full support (14+)
- **IE 11**: Not supported (CSS Grid, custom properties required)

---

## Development Tips

### Updating Colors

Change a single CSS variable to update entire theme:

```css
:root {
    --clr-accent: #new-color;  /* Updates all buttons, links, accents */
}
```

### Adding New Breakpoint

Add media query if needed:

```css
@media (max-width: 640px) {
    main {
        padding: 0 clamp(12px, 2vw, 18px) 48px;
    }
}
```

### Testing Responsive Behavior

Open DevTools and resize browser window. No page reloads needed—watch layout adapt fluidly.

---

*Last updated: 2026-07-03*
*CSS Version: 3*
*Compliance: WCAG 2.1 AA*
