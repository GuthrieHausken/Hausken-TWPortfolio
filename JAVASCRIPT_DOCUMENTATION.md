# JavaScript Documentation

## Overview
Inline script in `index.html` controls tabs, panel animations, side-nav sync, and preview modal behavior.

## DOM Collections
- `tabButtons`: `.tab-btn`
- `tabPanels`: `.tab-panel`
- `sideNavBtns`: `.side-nav-btn`
- `previewButtons`: `.preview-btn`
- Modal elements: `#pdf-modal`, `#pdf-frame`, `#image-preview`

## Core Functions

### `setActiveTab(targetId)`
- Stores the current window scroll position for the previously active tab
- Toggles `body.contact-active` when `targetId === 'contact'`
- Updates active state on top tabs (`.tab-btn`)
- Shows/hides tab panels using `hidden`
- Calls `animatePanelContent` for the active panel
- Syncs side-nav active state (`.side-nav-btn`)
- Restores the saved window scroll position for the newly active tab after it is shown

### `animatePanelContent(panel)`
- Targets revealable children via selector:
  - `.info-card, .sample-card, .cv-section, .work-item, .gallery-item`
- Clears and reapplies `.is-visible`
- Applies staggered `transitionDelay` for sequential entrance

### `openModal(filePath)`
- Detects image types with regex: `/\.(png|jpe?g|gif|webp|svg)$/i`
- Uses image element for images, iframe for PDFs
- Sets modal visible and disables body scrolling

### `closeModal()`
- Hides modal and clears preview sources
- Restores body scrolling

## Event Handling
- Tab click + keydown (Arrow, Home/End, Enter/Space)
- Side-nav button clicks
- Preview button clicks
- Modal close via close button, backdrop, and `Escape`

## Accessibility Behaviors
- Maintains `aria-selected` and tab `tabIndex`
- Keeps hidden panels out of normal flow and accessibility focus path
- Modal state mirrored via `aria-hidden`

Last updated: 2026-07-11
