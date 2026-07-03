# HTML Documentation - Portfolio Site V3

## Document Structure Overview

The `index.html` file represents a complete, semantically-structured portfolio website with accessibility-first implementation. The document follows a clear hierarchical structure optimized for both human readability and assistive technology interpretation.

---

## Document Head (`<head>`)

### Metadata & Encoding

```html
<meta charset="UTF-8">
```
- Declares UTF-8 character encoding for proper text rendering and internationalization support
- Must appear early in `<head>` (within first 1024 bytes) for browser recognition

### Viewport Configuration

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- Enables responsive design by setting viewport to device width
- `initial-scale=1.0` ensures no automatic zoom on mobile devices
- Critical for proper mobile rendering and touch interactions

### SEO Metadata

```html
<meta name="description" content="Portfolio site for Guthrie Hausken featuring writing samples, 
                                   editing experience, and a multi-tab layout.">
```
- Provides page description for search engine results
- Displayed in Google search snippets and social media shares
- Optimal length: 150-160 characters for full display in SERPs

### Document Title

```html
<title>Guthrie Hausken | Portfolio</title>
```
- Appears in browser tab and browser history
- Used as fallback for bookmarks and page references
- Format: `[Name] | [Site Type]` for clarity in search results

### Stylesheet Link

```html
<link rel="stylesheet" href="styles.css">
```
- Links external stylesheet with `rel="stylesheet"` for clarity
- `href` uses relative path (assumes `styles.css` in same directory)
- No `type="text/css"` attribute needed (implicit in HTML5)

---

## Accessibility Features in Head

### Skip Link

```html
<a class="skip-link" href="#main-content">Skip to main content</a>
```
- **Purpose**: Allows keyboard users to bypass navigation and header content
- **Implementation**: 
  - Hidden off-screen by default with negative `top` positioning
  - Appears on `:focus` for keyboard navigation
  - Links to `#main-content` to jump directly to main content
- **Best Practice**: Should be first focusable element on page
- **Visibility**: Visible to screen readers but hidden from sighted users (until focused)

---

## Main Content Structure

### Primary Container

```html
<main id="main-content">
```
- **Semantic Role**: `<main>` landmark provides navigation entry point for assistive technology
- **ID Attribute**: `id="main-content"` targeted by skip link
- **Accessibility Impact**: Screen readers identify this as primary content region
- **Document Outline**: Establishes main content section in document outline

---

## Header / Hero Section

### Hero Container

```html
<header class="hero">
    <div class="header-inner">
```

**Semantic Purpose**: 
- `<header>` element identifies introductory content at top of page
- `.hero` class applies distinctive visual styling (gradient background, border, shadow)
- `.header-inner` wrapper enables CSS Grid layout for responsive image + text positioning

### Profile Image

```html
<img src="Guthrie Hausken_02.jpg" 
     class="profile-img" 
     alt="Portrait of Guthrie Hausken">
```

**Image Best Practices**:
- **Alt Text**: Describes image content for screen readers ("Portrait of..." not just "Profile picture")
- **Filename**: Uses readable naming convention (spaces preserved, not encoded in HTML)
- **Path**: Relative path assumes image in same directory as HTML
- **Styling**: CSS handles sizing, border-radius, and responsive scaling
- **SEO**: Alt text helps image search indexing

### Header Content Container

```html
<div class="header-text">
```
- Non-semantic wrapper for grouping introduction text
- CSS handles max-width constraint and text layout
- Groups introduction paragraphs and tab buttons

### Eyebrow (Overline) Text

```html
<p class="eyebrow">Technical writing • editing • documentation</p>
```
- **Visual Hierarchy**: Positioned above main heading as introductory text
- **Styling**: Uppercase, reduced font-size, increased letter-spacing for prominence
- **Semantic**: Uses `<p>` tag (not `<span>`) for proper text node in document outline
- **Purpose**: Immediately communicates professional focus to visitors

### Main Heading

```html
<h1>Guthrie Hausken</h1>
```
- **Document Outline**: Primary heading (H1) — should appear once per page
- **Semantic Meaning**: Identifies page/site subject
- **SEO Value**: Page title used in search results and meta analysis
- **Styling**: Large, prominent typography via CSS scaling with `clamp()`

### Tagline

```html
<p class="tagline">Recent English graduate and technical writing student. 
                   Documentation system gremlin.</p>
```
- **Purpose**: Descriptive subtitle providing additional context
- **Semantic**: `<p>` for proper text flow (not divs)
- **Content**: Friendly, personality-driven description of expertise

---

## Tab Navigation System

### Tab Container (Tablist)

```html
<div class="hero-actions" role="tablist" aria-label="Portfolio sections">
```

**ARIA Implementation**:
- **`role="tablist"`**: Identifies container as WAI-ARIA tab component
- **`aria-label="Portfolio sections"`**: Labels the tab group for screen readers
- **Semantic Meaning**: Describes function of buttons within container
- **Accessibility**: Screen readers announce "Tab list, Portfolio sections" on focus

### Individual Tab Buttons

```html
<button class="tab-btn active" 
        type="button" 
        data-tab="overview" 
        id="overview-tab" 
        role="tab" 
        aria-controls="overview" 
        aria-selected="true" 
        tabindex="0">
    Overview
</button>
```

**Tab Button Anatomy**:

| Attribute | Purpose | Values |
|-----------|---------|--------|
| `class="tab-btn active"` | Visual styling; `.active` applied to current tab | "tab-btn" always; "active" conditionally |
| `type="button"` | Defines as clickable button (prevents form submission) | Always "button" |
| `data-tab="overview"` | Identifier linking button to content panel | Matches target panel's `#id` |
| `id="overview-tab"` | Unique identifier for ARIA reference | Semantic: `{name}-tab` pattern |
| `role="tab"` | ARIA role for tab component | Always "tab" |
| `aria-controls="overview"` | Links button to controlled element | References target panel `#id` |
| `aria-selected="true/false"` | Indicates active state | "true" for active tab, "false" for inactive |
| `tabindex="0/-1"` | Keyboard focus management | 0 = in tab order (active tab); -1 = out of tab order (inactive tabs) |

**Tab Button States**:

1. **Active Tab**: First button on page load
   - `class="tab-btn active"` 
   - `aria-selected="true"`
   - `tabindex="0"` (focusable)

2. **Inactive Tabs**: All other buttons
   - `class="tab-btn"` (no "active")
   - `aria-selected="false"`
   - `tabindex="-1"` (removed from tab order)

**Keyboard Navigation Supported**:
- Tab key: Moves focus between tab buttons
- Arrow keys: Cycle through tabs (Right/Down = next, Left/Up = previous)
- Home: Jump to first tab
- End: Jump to last tab
- Enter/Space: Activate current tab

---

## Tab Panels (Content Sections)

### Overview Panel (Active by Default)

```html
<section id="overview" 
         class="tab-panel active" 
         role="tabpanel" 
         aria-labelledby="overview-tab" 
         tabindex="0">
```

**Tab Panel Anatomy**:

| Attribute | Purpose |
|-----------|---------|
| `id="overview"` | Unique identifier; referenced by `aria-controls` in tab button |
| `class="tab-panel active"` | `.active` shows panel on load; `.tab-panel` applies styling |
| `role="tabpanel"` | ARIA role indicating content controlled by tab |
| `aria-labelledby="overview-tab"` | Links panel to its controlling tab button for screen readers |
| `tabindex="0"` | Allows panel itself to receive focus (optional but good practice) |

### Panel Introduction

```html
<div class="panel-intro">
    <h2>Overview</h2>
    <p>Human-centered documentation, anywhere it's needed.</p>
</div>
```

**Structure**:
- **Heading (H2)**: Section title; part of page outline (H1 → H2 hierarchy)
- **Paragraph**: Subtitle/description of section purpose
- **Wrapper Class**: `.panel-intro` enables consistent spacing across all tabs

### Info Cards Container

```html
<div class="card-grid overview-grid">
```

**Layout Pattern**:
- `.card-grid`: Base grid container (applies default grid layout)
- `.overview-grid`: Variant class overriding default to single-column layout
- CSS handles responsive column adjustments

### Info Card Component

```html
<article class="info-card">
    <h3>About</h3>
    <p>I'm a recent college graduate...</p>
    <h3>Mission statement</h3>
    <p>I will put this here later.</p>
</article>
```

**Card Structure**:
- **`<article>` Element**: Indicates self-contained, reusable content block
- **Headings (H3)**: Section headings within card (maintaining outline hierarchy)
- **Paragraphs**: Content text with semantic meaning
- **Styling**: `.info-card` class applies card appearance (padding, border, shadow, background)

---

## Writing Samples Section

### Portfolio Grid

```html
<div class="sample-grid">
```

**Purpose**: CSS Grid container for portfolio item cards
**Responsiveness**: Grid columns adjust based on viewport width via CSS

### Sample Card

```html
<article class="sample-card">
    <div class="sample-card-top">
        <div class="sample-icon">PDF</div>
        <div>
            <h3>Board Chair Description</h3>
            <p>A position description I volunteered to make...</p>
        </div>
    </div>
    <div class="sample-links">
        <button type="button" class="sample-btn preview-btn" data-pdf="%20Board%20Chair%20Description.pdf">
            Preview
        </button>
        <a href="%20Board%20Chair%20Description.pdf" target="_blank" rel="noopener">
            Open PDF
        </a>
        <a href="%20Board%20Chair%20Description.pdf" download>
            Download
        </a>
    </div>
</article>
```

**Card Components**:

#### Card Top Section
```html
<div class="sample-card-top">
```
- Flexbox container for icon + content layout
- Grows to fill available space within card

#### Icon Badge
```html
<div class="sample-icon">PDF</div>
```
- Visual indicator of file type
- CSS background color and styling make it prominent
- Improves scanability at a glance

#### Content Container
```html
<div>
    <h3>Board Chair Description</h3>
    <p>A position description...</p>
</div>
```
- Nested container for title and description
- Flex layout fills remaining space

#### Action Links Section
```html
<div class="sample-links">
```
- Groups three action buttons/links
- Flex layout distributes equally across width

#### Preview Button (Modal Trigger)
```html
<button type="button" class="sample-btn preview-btn" data-pdf="%20Board%20Chair%20Description.pdf">
    Preview
</button>
```
- **`type="button"`**: Prevents form submission
- **`.preview-btn`**: JavaScript hook for modal trigger functionality
- **`data-pdf`**: Stores file path; read by JavaScript `button.getAttribute('data-pdf')`
- **URL Encoding**: Spaces encoded as `%20` for proper URL handling
- **Functionality**: Clicking opens PDF in modal overlay

#### Open PDF Link
```html
<a href="%20Board%20Chair%20Description.pdf" target="_blank" rel="noopener">
    Open PDF
</a>
```
- **`href`**: Direct link to PDF file
- **`target="_blank"`**: Opens in new tab/window
- **`rel="noopener"`**: Security attribute preventing new page from accessing `window.opener`
- **Fallback**: Provides non-JavaScript access to file

#### Download Link
```html
<a href="%20Board%20Chair%20Description.pdf" download>
    Download
</a>
```
- **`download` Attribute**: Triggers browser download instead of navigation
- **No value**: Browser suggests filename from `href`
- **Fallback**: Alternative action if user wants local file

---

## Curriculum Vitae Section

### CV Container

```html
<article class="cv-sheet">
    <header class="cv-header">
```

**Semantic Structure**:
- **`<article>`**: CV is self-contained, reusable content
- **`<header>`**: Section for CV header/contact information

### CV Header

```html
<header class="cv-header">
    <div>
        <h3>Guthrie Hausken</h3>
        <p>Portland, Oregon | 541-224-2037 | guthriehausken@gmail.com</p>
    </div>
    <p class="cv-role">Emerging technical writer focused on human-centered design and documentation.</p>
</header>
```

**Sections**:
1. **Name**: H3 heading (maintains outline from H1 → H2 sections → H3 within sections)
2. **Contact Info**: Location, phone, email on single line with pipe separators
3. **Role Statement**: Professional summary positioned separately with `.cv-role` styling

### CV Sections

```html
<section class="cv-section">
    <h4>Professional Profile</h4>
    <p>Emerging technical writer with a B.A. in English...</p>
</section>
```

**Heading Hierarchy**: H4 used for CV subsections (H1 → H2 → H3 → H4 maintains proper nesting)

**Section Types**:

#### Professional Profile
- Summary paragraph describing background and expertise

#### Education
```html
<div class="cv-entry">
    <div class="cv-entry-head">
        <strong>Bachelor of Arts in English</strong>
        <span>University of Oregon</span>
    </div>
    <p>Cumulative GPA: 4.06</p>
</div>
```

- **CV Entry**: Grouped education item with institution and details
- **Entry Head**: Flex layout for degree title + institution side-by-side
- **Entry Content**: Details about the entry (GPA, dates, etc.)

#### Awards and Honors
```html
<ul>
    <li>2025 Stephen Swig Essay Prize, University of Oregon — $500</li>
    <li>Dean's List, University of Oregon — all semesters</li>
</ul>
```

- **List Structure**: Semantic `<ul>` for unordered awards
- **List Items**: Each award on separate line with em-dash for visual hierarchy

#### Experience Entries
```html
<div class="cv-entry">
    <div class="cv-entry-head">
        <strong>Writing Center Tutor</strong>
        <span>University Writing Centers, 2020–2023</span>
    </div>
    <ul>
        <li>Conducted one-on-one consultations...</li>
        <li>Edited and proofread academic documents...</li>
    </ul>
</div>
```

- **Job Title + Dates**: `<strong>` for emphasis + `<span>` for date range
- **Responsibilities**: Bulleted list for each position
- **Semantic**: Proper hierarchy with nested lists

#### Skills List
```html
<h4>Technical Skills</h4>
<ul>
    <li>Microsoft Word</li>
    <li>Adobe InDesign</li>
    <li>HTML and CSS</li>
</ul>
```

- Simple list of skills
- No special formatting needed

---

## Contact Section

### Contact Layout

```html
<div class="contact-layout">
    <article class="info-card">
        <h3>Reach out</h3>
        <ul class="contact-list">
```

**Structure**:
- `.contact-layout`: Grid wrapper for contact cards
- `.info-card`: Styled card container
- `.contact-list`: Styled unordered list for contact information

### Contact Links

```html
<li><a href="mailto:guthriehausken@gmail.com">✉ guthriehausken@gmail.com</a></li>
<li><a href="tel:+15412242037">☎ (541) 224-2037</a></li>
<li><a href="https://www.linkedin.com/in/guthrie-hausken-1b0534365/" target="_blank" rel="noopener noreferrer">
    ⟶ LinkedIn
</a></li>
```

**Link Types**:

| Type | Href Scheme | Purpose | Behavior |
|------|------------|---------|----------|
| Email | `mailto:` | Opens default email client | Not new window |
| Phone | `tel:` | Initiates phone call (mobile devices) | Device-dependent |
| External | `https://` | Links to external website | `target="_blank"` opens new window |

**Icon Elements**:
- **✉ (Mail symbol)**: Unicode character for visual enhancement
- **☎ (Phone symbol)**: Unicode character
- **⟶ (Arrow)**: Unicode character for external link indication
- **Purpose**: Visual indicators helping scannable interface

**External Link Security**:
```html
rel="noopener noreferrer"
```
- **`noopener`**: Prevents new page from accessing `window.opener`, stopping malicious scripts
- **`noreferrer`**: Prevents referrer information being sent to external site (privacy)
- **Best Practice**: Required for `target="_blank"` links to untrusted external sites

---

## Other Works Section

### Featured Works Container

```html
<div class="work-actions">
    <div class="work-item">
        <button type="button" class="sample-btn preview-btn" data-pdf="Rhetoric%20and%20Ethics%20Portfolio.pdf">
            Preview Rhetoric & Ethics Portfolio
        </button>
        <p class="work-caption">My Rhetoric and Ethics coursework portfolio.</p>
    </div>
</div>
```

**Work Item Structure**:
- **Button**: Preview trigger (same as portfolio section)
- **Caption**: Brief description of work item
- **Styling**: Flex column layout stacks button above caption

---

## Modal Overlay System

### Modal Container

```html
<div id="pdf-modal" class="pdf-modal" hidden aria-hidden="true">
    <div class="pdf-modal-backdrop" data-close-modal></div>
    <div class="pdf-modal-dialog" role="dialog" aria-modal="true" aria-label="Preview">
        <button type="button" class="pdf-modal-close" aria-label="Close preview" data-close-modal>×</button>
        <iframe id="pdf-frame" title="PDF preview" src="" hidden></iframe>
        <img id="image-preview" class="image-preview" alt="Preview" hidden>
    </div>
</div>
```

**Modal Components**:

#### Modal Container
- **`id="pdf-modal"`**: JavaScript hook for element selection
- **`hidden` Attribute**: Initially hidden; JavaScript removes this to show
- **`aria-hidden="true/false"`**: Screen reader visibility toggle (mirrors `hidden` attribute)

#### Backdrop
```html
<div class="pdf-modal-backdrop" data-close-modal></div>
```
- **Purpose**: Semi-transparent overlay behind modal content
- **`data-close-modal`**: JavaScript hook; clicking backdrop closes modal
- **Styling**: Full-screen overlay with click handler

#### Modal Dialog
```html
<div class="pdf-modal-dialog" role="dialog" aria-modal="true" aria-label="Preview">
```
- **`role="dialog"`**: ARIA role for modal dialog
- **`aria-modal="true"`**: Indicates modal behavior (focus trapped, background inert)
- **`aria-label="Preview"`**: Accessible name for screen readers

#### Close Button
```html
<button type="button" class="pdf-modal-close" aria-label="Close preview" data-close-modal>
    ×
</button>
```
- **Content**: × character (multiplication sign) as close icon
- **`aria-label`**: Descriptive label for screen readers (users don't read "×", they read "Close preview")
- **`data-close-modal`**: JavaScript hook for click handler

#### PDF Preview Frame
```html
<iframe id="pdf-frame" title="PDF preview" src="" hidden></iframe>
```
- **`<iframe>`**: Embeds PDF directly in page
- **`title="PDF preview"`**: Accessible name for iframe (screen readers announce this)
- **`id="pdf-frame"`**: JavaScript hook for src assignment
- **`src=""`: Initially empty; populated by JavaScript when preview opens
- **`hidden`**: Initially hidden; shown only when PDF preview needed

#### Image Preview
```html
<img id="image-preview" class="image-preview" alt="Preview" hidden>
```
- **`<img>`**: Image element for non-PDF previews (JPG, PNG, etc.)
- **`id="image-preview"`**: JavaScript hook for src assignment
- **`alt="Preview"`**: Accessible text for image (generic; not critical since decorative context)
- **`hidden`**: Initially hidden; shown only for image previews

---

## Footer

### Footer Element

```html
<footer>
    <p>&copy; 2026 Guthrie Hausken. All rights reserved.</p>
    <p>Built with semantic HTML, CSS, and JavaScript.</p>
</footer>
```

**Semantic Meaning**:
- **`<footer>` Element**: Landmark element containing page-level footer content
- **Content**: Copyright notice and site build information
- **Accessibility**: Footer landmark helps users navigate to bottom of page
- **Copyright Symbol**: `&copy;` HTML entity for © character

---

## JavaScript Integration

### Script Tag Location

```html
<script>
    // JavaScript code embedded directly in HTML
    // ...
</script>
```

**Best Practices**:
- Script placed at end of `<body>` (before `</body>` closing tag)
- Prevents render-blocking: DOM is parsed before script executes
- Variables and functions have access to all HTML elements
- No external JavaScript file needed (single file delivery reduces HTTP requests)

### JavaScript Functionality

The embedded `<script>` section manages:

1. **Tab Navigation**: Responds to click and keyboard events on tab buttons
2. **Tab Panel Visibility**: Shows/hides tab panels based on user selection
3. **Modal Preview System**: Opens PDFs/images in modal overlay
4. **Modal Dismissal**: Closes modal via button, backdrop click, or Escape key
5. **Keyboard Accessibility**: Arrow keys navigate tabs, arrow keys navigate modals

---

## HTML Validation Checklist

✓ Valid HTML5 semantic structure  
✓ Proper heading hierarchy (H1 → H2 → H3 → H4)  
✓ ARIA roles, labels, and state management  
✓ Keyboard navigation support  
✓ Skip link for keyboard users  
✓ Proper link semantics (mailto, tel, https)  
✓ Image alt text for all meaningful images  
✓ Semantic elements (`<main>`, `<header>`, `<section>`, `<article>`, `<footer>`)  
✓ Form controls have proper types (button, not div)  
✓ External links have `rel="noopener noreferrer"`  
✓ Meta charset and viewport declarations  
✓ Document title present  

---

## Common HTML Patterns Used

### Pattern 1: Tab Interface (ARIA Tabs)
Used for five-section navigation. Fully keyboard accessible with arrow key support.

### Pattern 2: Modal Dialog
Used for PDF/image previews. Traps focus and supports Escape key dismissal.

### Pattern 3: Card Component
Reusable `.info-card` and `.sample-card` for consistent content presentation.

### Pattern 4: Responsive Grid
Used for multi-column layouts that adapt to viewport width via CSS.

### Pattern 5: Document Hierarchy
Proper heading hierarchy (H1 → H2 → H3/H4) supports screen reader navigation and outline view.

---

*Last updated: 2026-07-03*
*HTML Version: 5*
*Compliance: WCAG 2.1 AA*
