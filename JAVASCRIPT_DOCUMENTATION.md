# JavaScript Documentation - Portfolio Site V3

## Overview

The JavaScript implementation uses **vanilla JavaScript** (no frameworks) to manage the interactive features of the portfolio site. The code provides two primary systems:

1. **Tab Navigation System**: Manages tab button clicks and keyboard navigation
2. **Modal Preview System**: Opens PDFs/images in an overlay modal

---

## Architecture

### Code Organization

The script is embedded directly in the HTML `<body>` at the end of the document. This approach:

- Reduces HTTP requests (no external file)
- Ensures DOM is fully loaded before script executes
- Maintains single-file delivery for portfolios
- Keeps related code together for portfolios

### Key Principles

1. **No External Dependencies**: Pure DOM manipulation
2. **Event Delegation**: Minimal event listeners (one per feature)
3. **Functional Programming**: Utility functions for reusable operations
4. **Accessibility First**: Keyboard navigation, ARIA management
5. **Progressive Enhancement**: Works without JavaScript; enhanced with it

---

## Tab Navigation System

### Overview

The tab system manages switching between five sections (Overview, Portfolio, Contact, CV, Other Works) with full keyboard accessibility.

### Data Structures

#### DOM Selection

```javascript
const tabButtons = document.querySelectorAll('.tab-btn');
const tabPanels = document.querySelectorAll('.tab-panel');
```

**Type**: `NodeList` (static collection of DOM elements)

**Content**:
- `tabButtons`: All button elements with `.tab-btn` class (5 buttons total)
- `tabPanels`: All section elements with `.tab-panel` class (5 sections total)

**Performance Note**: `querySelectorAll` creates static NodeList—most efficient for this use case

### Core Function: `setActiveTab(targetId)`

```javascript
function setActiveTab(targetId) {
    tabButtons.forEach((button) => {
        const isActive = button.getAttribute('data-tab') === targetId;
        button.classList.toggle('active', isActive);
        button.setAttribute('aria-selected', isActive ? 'true' : 'false');
        button.tabIndex = isActive ? 0 : -1;
    });

    tabPanels.forEach((panel) => {
        const isActive = panel.id === targetId;
        panel.hidden = !isActive;
        panel.classList.toggle('active', isActive);
    });
}
```

**Purpose**: Activates a specific tab and hides others

**Parameters**:
- `targetId` (String): ID of tab panel to activate (e.g., `"overview"`, `"writing"`)

**Logic Flow**:

1. **Update Tab Buttons**:
   - Loop through all tab buttons
   - Compare each button's `data-tab` attribute to target ID
   - Store result in `isActive` boolean
   - **`.classList.toggle('active', isActive)`**: Adds/removes `.active` class
     - Active tabs get `.active` (styled with gradient)
     - Inactive tabs lose `.active` (styled with white background)
   - **`aria-selected` Attribute**: Updates accessibility state
     - Active: `"true"` (screen reader announces selected)
     - Inactive: `"false"` (screen reader announces not selected)
   - **`tabIndex` Attribute**: Manages keyboard focus
     - Active: `0` (in tab order, focusable)
     - Inactive: `-1` (out of tab order, not focusable)

2. **Update Tab Panels**:
   - Loop through all tab panels
   - Compare each panel's `id` to target ID
   - **`panel.hidden` Property**: Controls visibility
     - Active panel: `hidden = false` (visible)
     - Inactive panels: `hidden = true` (hidden, removed from accessibility tree)
   - **`.classList.toggle('active', isActive)`**: Mirrors button state (CSS may style differently)

**Accessibility Impact**:
- Screen readers recognize only active tab and panel
- Keyboard users can only tab to active button
- ARIA attributes provide proper state feedback

### Click Event Handler

```javascript
tabButtons.forEach((button, index) => {
    button.addEventListener('click', () => {
        const targetId = button.getAttribute('data-tab');
        setActiveTab(targetId);
    });
```

**Trigger**: User clicks a tab button

**Flow**:
1. Retrieve `data-tab` attribute (contains target panel ID)
2. Call `setActiveTab()` with that ID
3. All panel switching happens via `setActiveTab()`

**User Experience**: Instant tab switching with immediate visual feedback

### Keyboard Navigation System

```javascript
    button.addEventListener('keydown', (event) => {
        const buttons = Array.from(tabButtons);
        let nextIndex = index;

        if (event.key === 'ArrowRight' || event.key === 'ArrowDown') {
            event.preventDefault();
            nextIndex = (index + 1) % buttons.length;
        } else if (event.key === 'ArrowLeft' || event.key === 'ArrowUp') {
            event.preventDefault();
            nextIndex = (index - 1 + buttons.length) % buttons.length;
        } else if (event.key === 'Home') {
            event.preventDefault();
            nextIndex = 0;
        } else if (event.key === 'End') {
            event.preventDefault();
            nextIndex = buttons.length - 1;
        } else if (event.key === 'Enter' || event.key === ' ') {
            event.preventDefault();
            setActiveTab(button.getAttribute('data-tab'));
            return;
        } else {
            return;
        }

        const nextButton = buttons[nextIndex];
        setActiveTab(nextButton.getAttribute('data-tab'));
        nextButton.focus();
    });
});
```

**Keyboard Shortcuts Supported**:

| Key | Action | Logic |
|-----|--------|-------|
| **→ / ↓** | Next tab | `(index + 1) % buttons.length` cycles forward |
| **← / ↑** | Previous tab | `(index - 1 + buttons.length) % buttons.length` cycles backward |
| **Home** | First tab | `nextIndex = 0` jumps to start |
| **End** | Last tab | `nextIndex = buttons.length - 1` jumps to end |
| **Enter** | Activate tab | Activates current tab (same as click) |
| **Space** | Activate tab | Activates current tab |

#### Modulo Arithmetic (Wrapping)

```javascript
nextIndex = (index + 1) % buttons.length;
```

**Example with 5 buttons** (indices 0-4):
- At button 0, press Right: `(0 + 1) % 5 = 1` → Move to button 1
- At button 4, press Right: `(4 + 1) % 5 = 0` → Wrap to button 0

**Backward with Offset**:
```javascript
nextIndex = (index - 1 + buttons.length) % buttons.length;
```

- At button 0, press Left: `(0 - 1 + 5) % 5 = 4` → Wrap to button 4
- At button 1, press Left: `(1 - 1 + 5) % 5 = 0` → Move to button 0

**Why Offset Needed**: JavaScript modulo with negative numbers returns negative results
- Without offset: `-1 % 5 = -1` (wrong)
- With offset: `(-1 + 5) % 5 = 4` (correct)

#### Focus Management

```javascript
nextButton.focus();
```

**Purpose**: Moves keyboard focus to selected button
**Effect**: Browser scrolls button into view if needed
**Accessibility**: Screen reader announces button name when focused

#### Event Prevention

```javascript
event.preventDefault();
```

**Purpose**: Stops default browser behavior
**Prevented**:
- Arrow keys: Default scroll behavior
- Home/End: Default jump-to-start/end behavior
- Enter/Space: Default form submission (if button in form)

---

## Modal Preview System

### Overview

The modal system displays PDFs (via iframe) and images (via img) in an overlay without leaving the page.

### Data Structures

#### DOM Selection

```javascript
const modal = document.getElementById('pdf-modal');
const frame = document.getElementById('pdf-frame');
const imagePreview = document.getElementById('image-preview');
const previewButtons = document.querySelectorAll('.preview-btn');
```

**Elements**:
- `modal`: Overlay container (backdrop + dialog)
- `frame`: iframe for PDF display
- `imagePreview`: img for image display
- `previewButtons`: All buttons with `.preview-btn` class

### Core Function: `closeModal()`

```javascript
function closeModal() {
    modal.hidden = true;
    modal.setAttribute('aria-hidden', 'true');
    frame.src = '';
    frame.hidden = true;
    imagePreview.src = '';
    imagePreview.hidden = true;
    document.body.style.overflow = '';
}
```

**Purpose**: Closes modal and resets state

**Steps**:

1. **Hide Modal**:
   - `modal.hidden = true` hides overlay from view
   - `aria-hidden="true"` removes modal from accessibility tree

2. **Clear iframe**:
   - `frame.src = ''` clears loaded PDF
   - `frame.hidden = true` hides iframe
   - Prevents cached PDF from lingering

3. **Clear Image**:
   - `imagePreview.src = ''` clears loaded image
   - `imagePreview.hidden = true` hides image
   - Prevents cached image from displaying

4. **Restore Page Scroll**:
   - `document.body.style.overflow = ''` restores default scrolling
   - Allows page scrolling again after modal blocked it

**Accessibility**: Screen readers no longer see modal content

### Core Function: `openModal(filePath)`

```javascript
function openModal(filePath) {
    const isImage = /\.(png|jpe?g|gif|webp|svg)$/i.test(filePath);

    if (isImage) {
        frame.hidden = true;
        imagePreview.hidden = false;
        imagePreview.src = filePath;
    } else {
        imagePreview.hidden = true;
        frame.hidden = false;
        frame.src = filePath;
    }

    modal.hidden = false;
    modal.setAttribute('aria-hidden', 'false');
    document.body.style.overflow = 'hidden';
}
```

**Purpose**: Opens modal with appropriate content type

**Parameters**:
- `filePath` (String): File path/URL to open (e.g., `"Rhetoric%20and%20Ethics%20Portfolio.pdf"`)

#### File Type Detection

```javascript
const isImage = /\.(png|jpe?g|gif|webp|svg)$/i.test(filePath);
```

**Regex Breakdown**:
```
/\.(png|jpe?g|gif|webp|svg)$/i
```

| Part | Meaning |
|------|---------|
| `/` | Start regex pattern |
| `\.` | Literal dot (escaped, since `.` is special in regex) |
| `(png\|jpe?g\|gif\|webp\|svg)` | Match any image extension |
| `png` | Literal "png" |
| `jpe?g` | "jpg" or "jpeg" (? = optional previous char) |
| `gif` | Literal "gif" |
| `webp` | Literal "webp" |
| `svg` | Literal "svg" |
| `$` | End of string (must end with image extension) |
| `i` | Case-insensitive flag (matches .PNG, .Jpg, etc.) |

**Examples**:
- `"Resume.pdf"` → `isImage = false` (displays in iframe)
- `"photo.jpg"` → `isImage = true` (displays in img)
- `"PHOTO.JPG"` → `isImage = true` (case-insensitive)
- `"document.jpeg"` → `isImage = true` (optional 'e' in "jpeg")

#### Content Type Routing

```javascript
if (isImage) {
    frame.hidden = true;           // Hide iframe (not needed)
    imagePreview.hidden = false;   // Show image element
    imagePreview.src = filePath;   // Set image source
} else {
    imagePreview.hidden = true;    // Hide image (not needed)
    frame.hidden = false;          // Show iframe
    frame.src = filePath;          // Set PDF source
}
```

**Routing Logic**:
- **Images**: Use `<img>` element (native browser support)
- **PDFs**: Use `<iframe>` (embeds PDF viewer)

**Why Separate Elements**: 
- Img elements can't display PDFs
- Iframes can display PDFs but poorly handle images
- Separate elements allow clean switching

#### Show Modal

```javascript
modal.hidden = false;
modal.setAttribute('aria-hidden', 'false');
document.body.style.overflow = 'hidden';
```

**Steps**:

1. **Reveal Modal**: `modal.hidden = false`
2. **Update Accessibility**: `aria-hidden="false"` makes modal visible to screen readers
3. **Block Page Scroll**: `overflow: 'hidden'` prevents scrolling behind modal
   - Improves UX: Can't scroll page while modal open
   - Reduces distraction: Focus stays on modal content

### Preview Button Click Handler

```javascript
previewButtons.forEach((button) => {
    button.addEventListener('click', () => {
        const pdfPath = button.getAttribute('data-pdf');
        openModal(pdfPath);
    });
});
```

**Trigger**: User clicks a preview button

**Flow**:
1. Get `data-pdf` attribute from button
2. Pass file path to `openModal()`
3. Modal displays appropriate content

**Flexibility**: Same handler works for any preview button by reading `data-pdf` attribute

---

## Modal Dismissal System

### Multiple Dismiss Methods

#### Method 1: Close Button

```javascript
document.querySelectorAll('[data-close-modal]').forEach((element) => {
    element.addEventListener('click', (event) => {
        event.preventDefault();
        event.stopPropagation();
        closeModal();
    });
});
```

**Selector**: `[data-close-modal]` targets any element with this attribute

**Elements Matched**:
- Close button (×)
- Backdrop (both have this attribute)

**Event Handling**:
- `event.preventDefault()` stops default link/button behavior
- `event.stopPropagation()` prevents event from bubbling to parent elements
- Prevents triggering parent event listeners accidentally

#### Method 2: Backdrop Click

```javascript
modal.addEventListener('click', (event) => {
    if (event.target === modal) {
        closeModal();
    }
});
```

**Trigger**: User clicks on modal backdrop

**Logic**:
- `event.target` is the element that received the click
- `modal` is the backdrop container
- Only close if user clicks exactly on backdrop (not modal content)
- Prevents accidental close when clicking buttons inside modal

**Event Delegation Pattern**: Listens on parent, checks target to determine action

#### Method 3: Escape Key

```javascript
document.addEventListener('keydown', (event) => {
    if (event.key === 'Escape' && !modal.hidden) {
        closeModal();
    }
});
```

**Trigger**: User presses Escape key while modal is open

**Conditions**:
- `event.key === 'Escape'` checks for Escape key
- `!modal.hidden` ensures modal is currently visible
- Prevents closing if modal already hidden (redundant but safe)

**Global Handler**: Listens on document, works from any focused element

---

## Best Practices Demonstrated

### 1. Event Delegation

```javascript
// Good: Single listener on parent
document.querySelectorAll('[data-close-modal]').forEach(el => {
    el.addEventListener('click', () => closeModal());
});

// Less efficient: Separate listener for each button
.pdf-modal-close.addEventListener('click', () => closeModal());
.pdf-modal-backdrop.addEventListener('click', () => closeModal());
```

**Benefit**: Fewer event listeners = better performance

### 2. Progressive Enhancement

```javascript
// HTML provides working links
<a href="file.pdf" download>Download</a>

// JavaScript adds modal preview
<button class="preview-btn" data-pdf="file.pdf">Preview</button>
```

**Benefit**: Site works without JavaScript; enhanced with it

### 3. ARIA Management

```javascript
// Update accessibility with state
button.setAttribute('aria-selected', isActive ? 'true' : 'false');
modal.setAttribute('aria-hidden', hidden ? 'true' : 'false');
```

**Benefit**: Screen readers provide accurate state information

### 4. Focus Management

```javascript
// Move focus to selected element
nextButton.focus();

// Track focus with tabindex
button.tabIndex = isActive ? 0 : -1;
```

**Benefit**: Keyboard users know where focus is; follows logical order

### 5. Event Prevention

```javascript
event.preventDefault();    // Stop default behavior
event.stopPropagation();   // Stop bubbling to parents
```

**Benefit**: Prevents unintended browser actions

---

## Performance Characteristics

### DOM Queries

```javascript
const tabButtons = document.querySelectorAll('.tab-btn');  // Runs once
tabButtons.forEach(...);  // Iterates 5 times (constant)
```

**Efficiency**: DOM queries run once at page load, stored in variables
- Not re-querying on every interaction
- Minimal performance impact

### Event Listeners

- **Tab clicks**: 5 listeners (one per button)
- **Tab keyboard**: 5 listeners (one per button)
- **Preview buttons**: N listeners (one per button)
- **Modal backdrops**: 1 listener (delegated)
- **Escape key**: 1 listener (global)

**Total**: ~12 listeners for entire site (negligible)

### State Changes

Each tab switch updates:
- 5 button classes
- 5 button aria-selected attributes
- 5 button tabindex values
- 5 panel hidden attributes
- 5 panel classes

**Frequency**: Only on user interaction (not continuous)

### Memory Usage

- All event listeners cleaned up when page unloads
- No memory leaks from circular references
- Clean DOM element references

---

## Browser Compatibility

| Feature | Min Version | Notes |
|---------|-------------|-------|
| `querySelectorAll` | IE8+ | Universal support |
| `classList` | IE10+ | Use `className` fallback if needed |
| `setAttribute` | IE6+ | Widely supported |
| Arrow keys in keyboard event | IE9+ | All modern browsers |
| RegExp test() | All | Standard JavaScript |

**Target**: Modern browsers (Chrome, Firefox, Safari, Edge latest)

---

## Debugging Tips

### Check Tab Status

```javascript
// In browser console
tabButtons.forEach(btn => {
    console.log(
        btn.textContent,
        btn.getAttribute('aria-selected'),
        btn.tabIndex
    );
});
```

**Output**: Shows which button is active

### Check Modal State

```javascript
// In browser console
console.log('Modal hidden:', modal.hidden);
console.log('Frame src:', frame.src);
console.log('Image src:', imagePreview.src);
```

**Output**: Confirms modal content loaded correctly

### Test Keyboard Navigation

1. Press Tab to focus first tab button
2. Press Arrow Right repeatedly → cycles through tabs
3. Press Home → jumps to first tab
4. Press End → jumps to last tab

### Test Modal Keyboard Dismissal

1. Click any preview button to open modal
2. Press Escape → modal should close
3. Try clicking outside modal → should close
4. Try clicking close button → should close

---

## Common Issues & Solutions

### Issue: Tab doesn't switch
**Cause**: Button's `data-tab` doesn't match panel's `id`
**Solution**: Check HTML attributes match exactly

### Issue: Modal won't open
**Cause**: File path invalid or missing
**Solution**: Check `data-pdf` attribute contains correct file path

### Issue: Keyboard navigation doesn't work
**Cause**: JavaScript error prevents execution
**Solution**: Check browser console for errors; verify JavaScript runs before interactions

### Issue: Modal won't close on Escape
**Cause**: Event listener not attached or event propagation stopped
**Solution**: Verify `keydown` listener on document; check `preventDefault` doesn't prevent Escape

---

## Future Enhancements

### Potential Improvements

1. **Tab State Persistence**
```javascript
// Save/load active tab from localStorage
localStorage.setItem('activeTab', targetId);
const savedTab = localStorage.getItem('activeTab');
```

2. **Animation Transitions**
```javascript
// Add fade/slide animations on tab switch
panel.classList.add('fade-in');
```

3. **Keyboard Shortcut Registry**
```javascript
// Configurable shortcuts for extensibility
const shortcuts = {
    'Ctrl+1': () => setActiveTab('overview'),
    'Ctrl+2': () => setActiveTab('writing'),
};
```

4. **History API Integration**
```javascript
// Browser back/forward works with tabs
window.history.pushState({ tab: targetId }, '', `#${targetId}`);
```

---

## Code Quality Checklist

✓ No external dependencies  
✓ Clean, readable variable names  
✓ Functions have single responsibility  
✓ Event handlers properly manage bubbling/capture  
✓ ARIA attributes maintained accurately  
✓ Keyboard navigation fully supported  
✓ No console errors or warnings  
✓ Minimal DOM queries (reused selectors)  
✓ Memory efficient (no memory leaks)  
✓ Works in all modern browsers  

---

*Last updated: 2026-07-03*
*JavaScript Version: ES6+*
*Framework: None (Vanilla)*
*Compliance: WCAG 2.1 AA*
