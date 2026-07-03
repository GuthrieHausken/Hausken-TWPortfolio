# Portfolio Site V3 - Developer Documentation

## Project Overview

**Portfolio Site V3** is a responsive, accessible portfolio website built with semantic HTML, modern CSS, and vanilla JavaScript. The site showcases Guthrie Hausken's work in technical writing, editing, and documentation through a tabbed interface with modal previews for PDF and image content.

### Key Features

- **Tabbed Navigation System**: Five primary sections (Overview, Portfolio, Contact, CV, Other Works) accessible via accessible tab controls
- **Modal Preview System**: In-page PDF and image preview capability without external navigation
- **Responsive Design**: Fluid typography and layout using CSS Grid and Flexbox with clamp() for scalable spacing
- **Accessibility First**: Semantic HTML, ARIA labels, keyboard navigation support (arrow keys, Tab, Enter, Escape)
- **Design System**: CSS custom properties (variables) for consistent theming and quick style updates
- **Performance Optimized**: Lightweight vanilla JavaScript with no external dependencies beyond fonts

### Technology Stack

- **HTML5**: Semantic markup with ARIA attributes for accessibility
- **CSS3**: Custom properties, Grid, Flexbox, gradients, and modern responsive techniques
- **JavaScript (Vanilla)**: No frameworks—pure DOM manipulation for tab switching and modal controls
- **Fonts**: Inter font family via system fallbacks (Segoe UI, Arial)

### File Structure

```
Portfolio Site V3/
├── index.html           # Main HTML document with semantic structure
├── styles.css           # Complete styling with design system tokens
├── README.md           # This file
└── [Media Assets]      # PNG, JPG, PDF files referenced in portfolio
    ├── Guthrie Hausken_02.jpg
    ├── Cat Manual.pdf
    ├── AI Report.pdf
    └── [Additional sample PDFs and images]
```

---

## Browser Support

- **Modern browsers**: Chrome, Firefox, Safari, Edge (latest versions)
- **CSS Features**: Grid, Flexbox, CSS variables, clamp(), backdrop-filter support recommended
- **JavaScript**: ES6 syntax (arrow functions, const/let, template literals)
- **Accessibility**: WCAG 2.1 AA compliant tab interface with proper ARIA annotations

---

## Design System

### Color Tokens (CSS Custom Properties)

```css
--clr-bg         /* Primary background color: #eef4fb (light blue) */
--clr-surface    /* Card/surface color: #ffffff (white) */
--clr-surface-2  /* Alternate surface: #f8fbff (very light blue) */
--clr-text       /* Primary text: #14233a (dark blue-gray) */
--clr-muted      /* Secondary text: #4d5f74 (muted gray-blue) */
--clr-line       /* Border/divider: #c7d5e6 (light blue-gray) */
--clr-accent     /* Primary accent: #123a64 (deep blue) */
--clr-accent-2   /* Secondary accent: #215290 (medium blue) */
--clr-accent-soft/* Soft accent background: #eaf3ff (very light blue) */
```

### Typography

- **Display Font**: Inter (fallback: Segoe UI, Arial)
- **Body Font**: Inter (fallback: Segoe UI, Arial)
- **Base Size**: 1rem (16px)
- **Line Height**: 1.7 (body), 1.6 (list items), 1.05 (headings)
- **Scale**: Uses clamp() for responsive sizing (e.g., `clamp(2rem, 4vw, 3rem)` for h1)

---

## Architecture & Design Patterns

### 1. Tab Interface Pattern

The site uses a **managed tab pattern** with state stored in DOM attributes and JavaScript control:

- Tab buttons control visibility of tab panels via `data-tab` attributes
- ARIA attributes (`role="tablist"`, `aria-selected`, `aria-controls`) ensure accessibility
- Keyboard navigation: Arrow keys traverse tabs, Enter/Space activates, Home/End jump to first/last
- Focus management: Only active tab button is in tab order (tabindex="0"), others have tabindex="-1"

### 2. Modal Overlay Pattern

The PDF/image preview system uses a **controlled modal pattern**:

- Single modal element (#pdf-modal) reused for all preview types
- Backdrop click, close button, and Escape key all trigger dismissal
- Detects file type (.jpg, .png, .gif, etc.) and routes to either iframe (PDFs) or img element
- Prevents page scroll when modal is open via `document.body.style.overflow = 'hidden'`

### 3. Responsive Layout Strategy

The site uses **flexible constraints** for responsive design:

- `min(100%, 1200px)` for max-width with fluid shrinking
- `clamp(min, preferred, max)` for responsive typography and spacing
- CSS Grid with `minmax(0, 1fr)` for flexible multi-column layouts
- Media query breakpoints handled naturally through intrinsic sizing

### 4. Visual Hierarchy

- **Hero section** (top priority): Large profile image, prominent heading, action buttons
- **Tab panels** (secondary): Organized content sections with clear hierarchy
- **Cards** (tertiary): Info cards, sample cards, and CV sections with consistent styling
- **Footer** (utility): Minimal, fixed at bottom

---

## Component Documentation

### Hero Section
- Displays user profile image, name, and navigation
- Uses CSS Grid: `grid-template-columns: auto minmax(0, 1fr)` for image + text layout
- Subtle background gradient and radial accent for visual depth
- Contains tab buttons for section navigation

### Tab System
- Five tabs: Overview, Portfolio, Contact, CV, Other Works
- Manages visibility of corresponding `.tab-panel` elements
- JavaScript handles all state management and user interactions

### Card Components
- `.info-card`: General content containers with consistent padding and borders
- `.sample-card`: Portfolio item cards with icon, title, description, and action buttons
- `.cv-sheet`: Full-width CV display with structured sections

### Modal Preview System
- Supports PDF previews via iframe and image previews via img element
- Single backdrop overlay prevents interaction with page content
- Smooth dismiss via click, button, or keyboard

---

## Key JavaScript Functions

### Tab Management

```javascript
setActiveTab(targetId)
```
- Updates all tab buttons: applies `.active` class, sets `aria-selected`, manages `tabIndex`
- Updates all tab panels: toggles `.hidden` attribute and `.active` class
- Ensures only one tab is active at a time

### Modal Management

```javascript
openModal(filePath)
```
- Detects file type (image vs PDF)
- Routes to appropriate preview element (iframe or img)
- Sets `modal.hidden = false` and prevents page scroll

```javascript
closeModal()
```
- Clears all preview sources
- Restores `modal.hidden = true` and page scrolling
- Resets modal state for next use

### Keyboard Navigation

Tab buttons support full keyboard accessibility:
- **Arrow Right/Down**: Move to next tab
- **Arrow Left/Up**: Move to previous tab
- **Home**: Jump to first tab
- **End**: Jump to last tab
- **Enter/Space**: Activate current tab

---

## Accessibility Features

### ARIA Implementation

- `role="tablist"` on `.hero-actions` container
- `role="tab"` on each `.tab-btn` with `aria-selected` reflecting active state
- `role="tabpanel"` on `.tab-panel` with `aria-labelledby` linking to tab button
- `role="dialog"`, `aria-modal="true"` on PDF modal for screen reader context
- `aria-hidden="true"` on decorative elements and hidden modals

### Semantic HTML

- Proper heading hierarchy (h1 > h2 > h3 > h4)
- `<section>` elements for major content areas
- `<article>` elements for card-based content
- `<button>` elements for interactive controls (not `<div>` or `<a>`)

### Keyboard Navigation

- Tab order managed with `tabindex="0"` and `tabindex="-1"`
- Skip link at top of page (`#main-content`) for keyboard users
- All interactive elements reachable via keyboard
- Modal dismissed with Escape key

### Skip Link

- Hidden off-screen by default
- Shows on focus (`:focus` state) above page content
- Links to `#main-content` ID to bypass navigation

---

## Performance Considerations

### Optimization Techniques

1. **No External Dependencies**: Vanilla JavaScript and system fonts reduce HTTP requests
2. **CSS Custom Properties**: Allow theme changes without CSS recompilation
3. **Efficient Grid Layouts**: Grid template calculations done once at render time
4. **Event Delegation**: Single event listeners on parent elements where possible
5. **Minimal Reflows**: State changes use class toggles rather than direct style manipulation
6. **Fixed Background**: `::before` pseudo-element with grid pattern fixed to viewport for parallelax effect without impact

### CSS Optimization

- Single stylesheet loaded once
- No CSS-in-JS runtime overhead
- Keyframe animations use `transform` and `opacity` for GPU acceleration
- Gradients pre-defined as custom properties for reusability

---

## Responsive Behavior

### Breakpoints (via intrinsic sizing, no media queries)

The design adapts fluidly through CSS functions:

- **Profile image**: `width: min(150px, 24vw)` scales down on smaller screens
- **Hero section padding**: `padding: clamp(36px, 6vw, 72px)` fluid between min/max
- **Main padding**: `padding: 0 clamp(18px, 4vw, 56px)` responsive horizontal spacing
- **H1 font size**: `font-size: clamp(2rem, 4vw, 3rem)` scales with viewport
- **Grid columns**: `grid-template-columns: repeat(2, minmax(0, 1fr))` wraps naturally on small screens

### Mobile Considerations

- Single-column layouts for card grids on narrow viewports
- Buttons stack horizontally with flex-wrap
- Touch targets sized appropriately (min 32px height per WCAG)
- No horizontal scrolling

---

## Usage Guide

### Adding a New Tab Section

1. Add a new button in `.hero-actions`:
```html
<button class="tab-btn" type="button" data-tab="new-section" id="new-section-tab" 
        role="tab" aria-controls="new-section" aria-selected="false" tabindex="-1">
  New Section
</button>
```

2. Add a corresponding tab panel:
```html
<section id="new-section" class="tab-panel" role="tabpanel" 
         aria-labelledby="new-section-tab" tabindex="0" hidden>
  <!-- Content here -->
</section>
```

3. JavaScript automatically handles the rest—no code changes needed!

### Previewing New PDFs

Add a preview button to any section:

```html
<button type="button" class="sample-btn preview-btn" data-pdf="path/to/file.pdf">
  Preview
</button>
```

The system automatically routes PDFs to iframe or images to img based on file extension.

### Updating Colors

Change any color by updating the CSS custom property in `:root`:

```css
:root {
    --clr-accent: #new-color;
    /* Updates all elements using this variable */
}
```

---

## Known Limitations & Future Improvements

### Current Limitations

1. **No localStorage**: Tab state is not persisted between page reloads
2. **Single modal**: Only one preview can be open at a time
3. **No lazy loading**: All PDFs are loaded into iframes when previewed
4. **Static content**: No backend integration for dynamic content

### Potential Enhancements

1. Add localStorage to remember user's last active tab
2. Implement image gallery with lightbox for multiple images
3. Add search functionality across portfolio items
4. Integrate contact form with backend validation
5. Add animations for tab transitions
6. Implement dark mode toggle

---

## Troubleshooting

### PDFs Not Displaying

- Verify PDF file paths are correct and URL-encoded (spaces as `%20`)
- Check browser console for CORS errors if loading from external source
- Some PDF viewers require specific iframe sandbox attributes

### Tab Not Switching

- Verify `data-tab` attribute matches the target `#id` of tab panel
- Check that JavaScript console has no errors
- Ensure tab button has `type="button"` to prevent form submission

### Accessibility Issues

- Use axe DevTools or WAVE to identify missing ARIA labels
- Verify heading hierarchy (no skipped levels)
- Test keyboard navigation with Tab, Arrow keys, and Escape

---

## Credits & Standards

- **HTML**: W3C semantic standards with ARIA 1.2
- **CSS**: Modern CSS 3 features with progressive enhancement
- **JavaScript**: ECMAScript 2015+ (ES6)
- **Accessibility**: WCAG 2.1 AA guidelines
- **Design**: Custom design system with mobile-first responsive approach

---

*Last updated: 2026-07-03*
*Version: 3.0*
